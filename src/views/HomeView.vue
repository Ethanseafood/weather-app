<template>
  <div class="home">
    <HelloWorld msg="台灣各地天氣預報" />
  </div>

  <div class="d-flex justify-content-center">
    <div class="recentWeather">
      <h3>今日白天</h3>
      <h5>{{ todayMinT }}-{{ todayMaxT }}°C</h5>
      <h5>降雨機率: {{ todayPop }}%</h5>
      <h5>{{ todayCi }}</h5>
    </div>
    <div class="recentWeather">
      <h3>今晚明晨</h3>
      <h5>{{ tonightMinT }}-{{ tonightMaxT }}°C</h5>
      <h5>降雨機率: {{ tonightPop }}%</h5>
      <h5>{{ tonightCi }}</h5>
    </div>
    <div class="recentWeather">
      <h3>明日白天</h3>
      <h5>{{ tomorrowMinT }}-{{ tomorrowMaxT }}°C</h5>
      <h5>降雨機率: {{ tomorrowPop }}%</h5>
      <h5>{{ tomorrowCi }}</h5>
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
    <button @click="fetchTwoDaysWeather">確定</button>
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
      selectedCity: "臺北市",
      todayCi: "",
      tonightCi: "",
      tomorrowCi: "",
      todayPop: "",
      tonightPop: "",
      tomorrowPop: "",
      todayMinT: "",
      tonightMinT: "",
      tomorrowMinT: "",
      todayMaxT: "",
      tonightMaxT: "",
      tomorrowMaxT: "",
    };
  },
  methods: {
    async fetchTwoDaysWeather() {
      try {
        const response = await axios.get(
          "https://opendata.cwb.gov.tw/api/v1/rest/datastore/F-C0032-001",
          {
            params: {
              Authorization: "CWB-81D86098-6243-4CC9-9ED2-12EC44900410",
              format: "JSON",
              locationName: this.selectedCity,
              elementName: "",
            },
          }
        );
        console.log(response.data);
        // console.log(
        //   response.data.records.location[0].weatherElement[1].time[0].parameter
        //     .parameterName
        // );
        this.todayCi =
          response.data.records.location[0].weatherElement[3].time[0].parameter.parameterName;
        this.tonightCi =
          response.data.records.location[0].weatherElement[3].time[1].parameter.parameterName;
        this.tomorrowCi =
          response.data.records.location[0].weatherElement[3].time[2].parameter.parameterName;

        this.todayPop =
          response.data.records.location[0].weatherElement[1].time[0].parameter.parameterName;
        this.tonightPop =
          response.data.records.location[0].weatherElement[1].time[1].parameter.parameterName;
        this.tomorrowPop =
          response.data.records.location[0].weatherElement[1].time[2].parameter.parameterName;

        this.todayMinT =
          response.data.records.location[0].weatherElement[2].time[0].parameter.parameterName;
        this.tonightMinT =
          response.data.records.location[0].weatherElement[2].time[1].parameter.parameterName;
        this.tomorrowMinT =
          response.data.records.location[0].weatherElement[2].time[2].parameter.parameterName;

        this.todayMaxT =
          response.data.records.location[0].weatherElement[4].time[0].parameter.parameterName;
        this.tonightMaxT =
          response.data.records.location[0].weatherElement[4].time[1].parameter.parameterName;
        this.tomorrowMaxT =
          response.data.records.location[0].weatherElement[4].time[2].parameter.parameterName;
      } catch (error) {
        console.error(error);
      }
    },
  },
  mounted() {
    this.fetchTwoDaysWeather();
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
