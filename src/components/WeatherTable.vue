<template>
  <table class="table table-bordered">
    <thead>
      <tr>
        <th>{{ selectedCity }}</th>
        <th v-for="(header, index) in tableHeaders" :key="index">
          <div v-html="header"></div>
        </th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>白天</td>
        <td v-for="index in 7" :key="index">
          {{ minTValues[index * 2 - 2] }}-{{ maxTValues[index * 2 - 2] }}°C
        </td>
      </tr>
      <tr>
        <td>晚上</td>
        <td v-for="index in 7" :key="index">
          {{ minTValues[index * 2 - 1] }}-{{ maxTValues[index * 2 - 1] }}°C
        </td>
      </tr>
      <tr>
        <td>體感溫度</td>
        <td v-for="index in 7" :key="index">
          {{ minATValues[index * 2 - 2] }}-{{ maxATValues[index * 2 - 2] }}°C
        </td>
      </tr>

      <tr>
        <td>紫外線</td>
        <td v-for="(value, index) in uviValues" :key="index">{{ value }}</td>
      </tr>
    </tbody>
  </table>
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
