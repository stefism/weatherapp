<!-- eslint-disable vue/multi-word-component-names -->
<template>
  <div>
    <h4>Weather</h4>
    <h4>Weather</h4>
    <h4>Weather</h4>
    <h4>Weather</h4>
    <h4>Weather</h4>
  </div>
</template>

<script>
import axios from "axios";

export default {
  // eslint-disable-next-line vue/multi-word-component-names
  name: "Weather",
  props: ["cities"],
  data() {
    return {
      APIKey: process.env.VUE_APP_OPENWEATHER_API_KEY,
      forecast: null,
      currentWeather: null,
      loading: true,
    };
  },
  created() {
    // console.log("this.cities - weather", this.cities);
    this.getWeather();
  },
  methods: {
    getWeather() {
      const currentCity = this.cities.find(
        (c) => c.name == this.$route.params.city
      );
      console.log("currentCity", currentCity);
      axios
        .get(
          `https://api.openweathermap.org/data/3.0/onecall?lat=${currentCity.coord.lat}&lon=${currentCity.coord.lon}&exclude={part}&appid=${this.APIKey}`
        )
        .then((responce) => {
          this.forecast = responce.data;
        })
        .then(() => {
          this.loading = false;
          console.log("this.forecast", this.forecast);
        });
    },
  },
};
</script>

<style></style>
