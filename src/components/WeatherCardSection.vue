<template>
  <div class="d-flex justify-content-center">
    <WeatherCard
      title="今日白天"
      :minT="minTValues[0]"
      :maxT="maxTValues[0]"
      :pop="popValues[0]"
      :ci="ciValues[0]"
    />
    <WeatherCard
      title="今晚明晨"
      :minT="minTValues[1]"
      :maxT="maxTValues[1]"
      :pop="popValues[1]"
      :ci="ciValues[1]"
    />
    <WeatherCard
      title="明日白天"
      :minT="minTValues[2]"
      :maxT="maxTValues[2]"
      :pop="popValues[2]"
      :ci="ciValues[2]"
    />
  </div>
</template>

<script>
import WeatherCard from "@/components/WeatherCard.vue";

export default {
  components: {
    WeatherCard,
  },
  props: {
    minTValues: Array,
    maxTValues: Array,
    popValues: Array,
    ciValues: Array,
  },
};
</script>
