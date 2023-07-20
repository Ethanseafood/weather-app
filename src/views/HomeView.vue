<template>
  <div class="home">
    <HelloWorld msg="台灣各地天氣預報" />
    <div class="d-flex justify-content-center">
      <div class="recentWeather">
        <h3>今日白天</h3>
        <h5>{{ minTValues[0] }}-{{ maxTValues[0] }}°C</h5>
        <h5>降雨機率: {{ popValues[0] }}%</h5>
        <h5>{{ ciValues[0] }}</h5>
      </div>
      <div class="recentWeather">
        <h3>今晚明晨</h3>
        <h5>{{ minTValues[1] }}-{{ maxTValues[1] }}°C</h5>
        <h5>降雨機率: {{ popValues[1] }}%</h5>
        <h5>{{ ciValues[1] }}</h5>
      </div>
      <div class="recentWeather">
        <h3>明日白天</h3>
        <h5>{{ minTValues[2] }}-{{ maxTValues[2] }}°C</h5>
        <h5>降雨機率: {{ popValues[2] }}%</h5>
        <h5>{{ ciValues[2] }}</h5>
      </div>
    </div>

    <div>
      <label for="citySelect">選擇縣市:</label>
      <select v-model="selectedCity" id="citySelect">
        <option disabled value="">請選擇縣市</option>
        <option value="基隆市">基隆市</option>
        <option value="臺北市">臺北市</option>
        <option value="新北市">新北市</option>
        <option value="桃園市">桃園市</option>
        <option value="新竹市">新竹市</option>
        <option value="新竹縣">新竹縣</option>
        <option value="苗栗縣">苗栗縣</option>
        <option value="臺中市">臺中市</option>
        <option value="彰化縣">彰化縣</option>
        <option value="南投縣">南投縣</option>
        <option value="雲林縣">雲林縣</option>
        <option value="嘉義市">嘉義市</option>
        <option value="嘉義縣">嘉義縣</option>
        <option value="臺南市">臺南市</option>
        <option value="高雄市">高雄市</option>
        <option value="屏東縣">屏東縣</option>
        <option value="宜蘭縣">宜蘭縣</option>
        <option value="花蓮縣">花蓮縣</option>
        <option value="臺東縣">臺東縣</option>
        <option value="澎湖縣">澎湖縣</option>
        <option value="金門縣">金門縣</option>
        <option value="連江縣">連江縣</option>
      </select>
      <button @click="updateCity">確定</button>
    </div>
    <div class="d-flex justify-content-center">
      <table class="table table-bordered">
        <thead>
          <tr>
            <th>{{ currentCity }}</th>
            <th v-for="(header, index) in tableHeaders" :key="index">
              <div v-html="header"></div>
            </th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td>白天</td>
            <td>{{ minTValues[0] }}-{{ maxTValues[0] }}°C</td>
            <td>{{ minTValues[2] }}-{{ maxTValues[2] }}°C</td>
            <td>{{ minTValues[4] }}-{{ maxTValues[4] }}°C</td>
            <td>{{ minTValues[6] }}-{{ maxTValues[6] }}°C</td>
            <td>{{ minTValues[8] }}-{{ maxTValues[8] }}°C</td>
            <td>{{ minTValues[10] }}-{{ maxTValues[10] }}°C</td>
            <td>{{ minTValues[12] }}-{{ maxTValues[12] }}°C</td>
          </tr>
          <tr>
            <td>晚上</td>
            <td>{{ minTValues[1] }}-{{ maxTValues[1] }}°C</td>
            <td>{{ minTValues[3] }}-{{ maxTValues[3] }}°C</td>
            <td>{{ minTValues[5] }}-{{ maxTValues[5] }}°C</td>
            <td>{{ minTValues[7] }}-{{ maxTValues[7] }}°C</td>
            <td>{{ minTValues[9] }}-{{ maxTValues[9] }}°C</td>
            <td>{{ minTValues[11] }}-{{ maxTValues[11] }}°C</td>
            <td>{{ minTValues[13] }}-{{ maxTValues[13] }}°C</td>
          </tr>
          <tr>
            <td>體感溫度</td>
            <td>{{ minATValues[0] }}-{{ maxATValues[0] }}°C</td>
            <td>{{ minATValues[2] }}-{{ maxATValues[2] }}°C</td>
            <td>{{ minATValues[4] }}-{{ maxATValues[4] }}°C</td>
            <td>{{ minATValues[6] }}-{{ maxATValues[6] }}°C</td>
            <td>{{ minATValues[8] }}-{{ maxATValues[8] }}°C</td>
            <td>{{ minATValues[10] }}-{{ maxATValues[10] }}°C</td>
            <td>{{ minATValues[12] }}-{{ maxATValues[12] }}°C</td>
          </tr>
          <tr>
            <td>紫外線</td>
            <td>{{ uviValues[0] }}</td>
            <td>{{ uviValues[1] }}</td>
            <td>{{ uviValues[2] }}</td>
            <td>{{ uviValues[3] }}</td>
            <td>{{ uviValues[4] }}</td>
            <td>{{ uviValues[5] }}</td>
            <td>{{ uviValues[6] }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from "@/components/HelloWorld.vue";
import axios from "axios";

export default {
  name: "HomeView",
  components: {
    HelloWorld,
  },
  data() {
    return {
      tableHeaders: [],
      selectedCity: "臺北市",
      currentCity: "臺北市",
      ciValues: [],
      minTValues: [],
      maxTValues: [],
      minATValues: [],
      maxATValues: [],
      uviValues: [],
      popValues: [],
    };
  },
  methods: {
    fetchData() {
      // 首先清空所有數據，避免頁面顯示舊的數據
      this.ciValues = [];
      this.minTValues = [];
      this.maxTValues = [];
      this.minATValues = [];
      this.maxATValues = [];
      this.uviValues = [];
      this.popValues = [];

      // 然後再重新抓取新的數據
      this.fetchTwoDaysCI();
      this.fetchTwoDaysPOP();
      this.fetchSevendaysMinT();
      this.fetchSevendaysMaxT();
      this.fetchSevendaysMinAT();
      this.fetchSevendaysMaxAT();
      this.fetchSevendaysUVI();
    },
    updateCity() {
      // 當按鈕點擊時更新當前縣市，但不改變選項選擇
      this.currentCity = this.selectedCity;
      // 然後再重新抓取數據
      this.fetchData();
    },
    generateTableHeaders() {
      const currentDate = new Date();
      for (let i = 0; i < 7; i++) {
        const date = new Date(currentDate.getTime() + i * 24 * 60 * 60 * 1000);
        const month = date.getMonth() + 1;
        const day = date.getDate();
        const weekday = this.getWeekday(date.getDay());
        const header = `${month}/${day}<br>${weekday}`;
        this.tableHeaders.push(header);
      }
    },
    getWeekday(dayIndex) {
      const weekdays = [
        "星期日",
        "星期一",
        "星期二",
        "星期三",
        "星期四",
        "星期五",
        "星期六",
      ];
      return weekdays[dayIndex];
    },
    async fetchTwoDaysCI() {
      try {
        const response = await axios.get(
          "https://opendata.cwb.gov.tw/api/v1/rest/datastore/F-C0032-001",
          {
            params: {
              Authorization: "CWB-81D86098-6243-4CC9-9ED2-12EC44900410",
              format: "JSON",
              locationName: this.selectedCity,
              elementName: "CI",
            },
          }
        );

        const ciData = response.data.records.location[0].weatherElement[0].time;
        const ciValues = ciData.map((time) => time.parameter.parameterName);
        this.ciValues = ciValues;
        console.log("ciValues", ciValues);
      } catch (error) {
        console.error(error);
      }
    },
    async fetchTwoDaysPOP() {
      try {
        const response = await axios.get(
          "https://opendata.cwb.gov.tw/api/v1/rest/datastore/F-C0032-001",
          {
            params: {
              Authorization: "CWB-81D86098-6243-4CC9-9ED2-12EC44900410",
              format: "JSON",
              locationName: this.selectedCity,
              elementName: "PoP",
            },
          }
        );

        const popData =
          response.data.records.location[0].weatherElement[0].time;

        const popValues = popData.map((time) =>
          Number(time.parameter.parameterName)
        );
        this.popValues = popValues;
        console.log("popValues", popValues);
      } catch (error) {
        console.error(error);
      }
    },

    async fetchSevendaysMinT() {
      try {
        const response = await axios.get(
          "https://opendata.cwb.gov.tw/api/v1/rest/datastore/F-D0047-091",
          {
            params: {
              Authorization: "CWB-81D86098-6243-4CC9-9ED2-12EC44900410",
              format: "JSON",
              locationName: this.selectedCity,
              elementName: "MinT",
            },
          }
        );

        // 提取 MinT 数据
        const minTData =
          response.data.records.locations[0].location[0].weatherElement[0].time;

        // 处理数据，将 MinT 值存储在数组中
        const minTValues = minTData.map((time) =>
          Number(time.elementValue[0].value)
        );
        this.minTValues = minTValues; // 将获取到的 MinT 数据赋值给 data 中的 minTValues
        console.log("weekMinT", minTValues);
      } catch (error) {
        console.error(error);
      }
    },
    async fetchSevendaysMaxT() {
      try {
        const response = await axios.get(
          "https://opendata.cwb.gov.tw/api/v1/rest/datastore/F-D0047-091",
          {
            params: {
              Authorization: "CWB-81D86098-6243-4CC9-9ED2-12EC44900410",
              format: "JSON",
              locationName: this.selectedCity,
              elementName: "MaxT",
            },
          }
        );

        const maxTData =
          response.data.records.locations[0].location[0].weatherElement[0].time;

        const maxTValues = maxTData.map((time) =>
          Number(time.elementValue[0].value)
        );
        this.maxTValues = maxTValues;
        console.log("weekMaxT", maxTValues);
      } catch (error) {
        console.error(error);
      }
    },
    async fetchSevendaysMinAT() {
      try {
        const response = await axios.get(
          "https://opendata.cwb.gov.tw/api/v1/rest/datastore/F-D0047-091",
          {
            params: {
              Authorization: "CWB-81D86098-6243-4CC9-9ED2-12EC44900410",
              format: "JSON",
              locationName: this.selectedCity,
              elementName: "MinAT",
            },
          }
        );

        const minATData =
          response.data.records.locations[0].location[0].weatherElement[0].time;
        const minATValues = minATData.map((time) =>
          Number(time.elementValue[0].value)
        );
        this.minATValues = minATValues;
        console.log("weekMinAT", minATValues);
      } catch (error) {
        console.error(error);
      }
    },
    async fetchSevendaysMaxAT() {
      try {
        const response = await axios.get(
          "https://opendata.cwb.gov.tw/api/v1/rest/datastore/F-D0047-091",
          {
            params: {
              Authorization: "CWB-81D86098-6243-4CC9-9ED2-12EC44900410",
              format: "JSON",
              locationName: this.selectedCity,
              elementName: "MaxAT",
            },
          }
        );

        const maxATData =
          response.data.records.locations[0].location[0].weatherElement[0].time;
        const maxATValues = maxATData.map((time) =>
          Number(time.elementValue[0].value)
        );
        this.maxATValues = maxATValues;
        console.log("weekMaxAT", maxATValues);
      } catch (error) {
        console.error(error);
      }
    },
    async fetchSevendaysUVI() {
      try {
        const response = await axios.get(
          "https://opendata.cwb.gov.tw/api/v1/rest/datastore/F-D0047-091",
          {
            params: {
              Authorization: "CWB-81D86098-6243-4CC9-9ED2-12EC44900410",
              format: "JSON",
              locationName: this.selectedCity,
              elementName: "UVI",
            },
          }
        );

        const uviData =
          response.data.records.locations[0].location[0].weatherElement[0].time;
        const uviValues = uviData.map((time) =>
          Number(time.elementValue[0].value)
        );
        this.uviValues = uviValues;
        console.log("weekUVI", uviValues);
      } catch (error) {
        console.error(error);
      }
    },
  },
  mounted() {
    this.generateTableHeaders();
    this.fetchTwoDaysCI();
    this.fetchTwoDaysPOP();
    this.fetchSevendaysMinT();
    this.fetchSevendaysMaxT();
    this.fetchSevendaysMinAT();
    this.fetchSevendaysMaxAT();
    this.fetchSevendaysUVI();
  },
};
</script>

<style scoped>
.recentWeather {
  background-color: lightgray;
  margin-right: 10px;
  padding: 10px;
}
</style>
