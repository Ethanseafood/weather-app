<template>
  <div class="table-container">
    <table class="table table-bordered">
      <thead>
        <tr>
          <th class="tiffany-blue-header">{{ selectedCity }}</th>
          <th
            v-for="(header, index) in tableHeaders"
            :key="index"
            class="tiffany-blue-header"
          >
            <div v-html="header"></div>
          </th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td class="tiffany-blue-header">白天</td>
          <td v-for="index in 7" :key="index">
            {{ minTValues[index * 2 - 2] }}-{{ maxTValues[index * 2 - 2] }}°C
          </td>
        </tr>
        <tr>
          <td class="tiffany-blue-header">晚上</td>
          <td v-for="index in 7" :key="index">
            {{ minTValues[index * 2 - 1] }}-{{ maxTValues[index * 2 - 1] }}°C
          </td>
        </tr>
        <tr>
          <td class="tiffany-blue-header">體感溫度</td>
          <td v-for="index in 7" :key="index">
            {{ minATValues[index * 2 - 2] }}-{{ maxATValues[index * 2 - 2] }}°C
          </td>
        </tr>

        <tr>
          <td class="tiffany-blue-header">紫外線</td>
          <td v-for="(value, index) in uviValues" :key="index">{{ value }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  props: {
    selectedCity: String,
    tableHeaders: Array,
    minTValues: Array,
    maxTValues: Array,
    minATValues: Array,
    maxATValues: Array,
    uviValues: Array,
  },
};
</script>
<style scoped>
.table-container {
  width: 80%;
  margin: 0 auto; /* This centers the element horizontally */
}
.tiffany-blue-header {
  background-color: #0abab5; /* Tiffany Blue color code */
  color: white; /* Text color, you can adjust this based on your needs */
}
</style>
