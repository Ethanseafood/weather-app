<template>
  <div class="recentWeather">
    <h3>{{ title }}</h3>
    <h5>{{ minT }}-{{ maxT }}°C</h5>
    <h5>降雨機率: {{ pop }}%</h5>
    <h5>{{ ci }}</h5>
  </div>
</template>

<script>
export default {
  props: {
    title: String,
    minT: Number,
    maxT: Number,
    pop: Number,
    ci: String,
  },
};
</script>
<style scoped>
.recentWeather {
  background-color: lightgray;
  margin-right: 10px;
  padding: 10px;
  border-radius: 10px;
}
</style>
