<template>
  <div>
    <label for="citySelect">選擇縣市:</label>
    <select v-model="selectedCity" id="citySelect" class="select-dropdown">
      <option disabled value="">請選擇縣市</option>
      <!-- 縣市選項列表 -->
      <option v-for="city in cities" :key="city" :value="city">
        {{ city }}
      </option>
    </select>
    <button class="btn" @click="updateCity">確定</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      selectedCity: "臺北市",
      cities: [
        "基隆市",
        "臺北市",
        "新北市",
        "桃園市",
        "新竹市",
        "新竹縣",
        "苗栗縣",
        "臺中市",
        "彰化縣",
        "南投縣",
        "雲林縣",
        "嘉義市",
        "嘉義縣",
        "臺南市",
        "高雄市",
        "屏東縣",
        "宜蘭縣",
        "花蓮縣",
        "臺東縣",
        "澎湖縣",
        "金門縣",
        "連江縣",
      ],
    };
  },
  methods: {
    updateCity() {
      // 將選擇的縣市進行更新
      this.$emit("update-city", this.selectedCity);
    },
  },
};
</script>
<style scoped>
.select-dropdown,
.btn {
  margin: 10px;
  padding: 10px;
  font-size: 16px;
}
.btn {
  background-color: #808080; /* Gray background color */
  color: #ffffff; /* White text color */
  border: none;
}
.btn:hover {
  background-color: rgb(151, 151, 151); /* Darker gray on hover */
}
</style>
