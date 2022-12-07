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
    // this.getWeather();
    this.getCurrentTime();
  },
  methods: {
    getCurrentTime() {
      const dateObject = new Date();
      const currentTime = dateObject.getHours();

      const currentCity = this.cities.find(
        (c) => c.name == this.$route.params.city
      );

      if(currentCity == undefined) {
        this.$router.push("/");
        return;
      }

      const sunrise = new Date(currentCity.sys.sunrise * 1000).getHours();
      const sunset = new Date(currentCity.sys.sunset * 1000).getHours();

      if(currentTime > sunrise && currentTime < sunset) {
        this.$emit("is-day", true);
      } else {
        this.$emit("is-day", false);
      }
    },
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
