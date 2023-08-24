<template>
  <div class="home">
    <h1>台灣各地天氣預報</h1>
    <!-- weahterCard -->
    <WeatherCardSection
      :minTValues="minTValues"
      :maxTValues="maxTValues"
      :popValues="popValues"
      :ciValues="ciValues"
    />
    <!-- citySelector -->
    <CitySelector @update-city="updateCity" />

    <!-- weatherTable -->
    <WeatherTable
      :selectedCity="selectedCity"
      :tableHeaders="tableHeaders"
      :minTValues="minTValues"
      :maxTValues="maxTValues"
      :minATValues="minATValues"
      :maxATValues="maxATValues"
      :uviValues="uviValues"
    />
  </div>
</template>

<script>
// @ is an alias to /src
import axios from "axios";
import WeatherCardSection from "@/components/WeatherCardSection.vue";
import CitySelector from "@/components/CitySelector.vue";
import WeatherTable from "@/components/WeatherTable.vue";
export default {
  name: "HomeView",
  components: {
    WeatherCardSection,
    CitySelector,
    WeatherTable,
  },
  data() {
    return {
      tableHeaders: [],
      selectedCity: "臺北市",
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
    updateCity(selectedCity) {
      // 當按鈕點擊時更新當前縣市，但不改變選項選擇
      this.selectedCity = selectedCity;
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
