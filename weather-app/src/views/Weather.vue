<!-- eslint-disable vue/multi-word-component-names -->
<template>
  <div class="main">
    <div v-if="loading" class="loading">
      <span></span>
    </div>
    <div v-else class="weather" :class="{ day: isDay, night: !isDay }">
      <div class="weather-wrap">
        <CurrentWeather :isDay="isDay" :currentWeather="currentWeather" />
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import CurrentWeather from "@/components/CurrentWeather.vue";

export default {
  // eslint-disable-next-line vue/multi-word-component-names
  name: "Weather",
  props: ["cities", "isDay"],
  components: { CurrentWeather },
  data() {
    return {
      APIKey: process.env.VUE_APP_API_EXPLORER_API_KEY,
      forecast: null,
      currentWeather: null,
      loading: true,
    };
  },
  created() {
    // console.log("this.cities - weather", this.cities);
    this.getCurrentTime();
    this.getWeather();
  },
  methods: {
    getCurrentTime() {
      const dateObject = new Date();
      const currentTime = dateObject.getHours();

      const currentCity = this.cities.find(
        (c) => c.name == this.$route.params.city
      );

      console.log("currentCity", currentCity);

      if(currentCity == undefined) {
        this.$router.push("/");
        return;
      }

      this.currentWeather = currentCity;

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

      if(currentCity == undefined) {
        return;
      }
      
      axios
        .get(
          `http://api.weatherapi.com/v1/forecast.json?key=${this.APIKey}&q=${currentCity.name}&days=3&lang=bg&aqi=yes&alerts=yes`
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

<style lang="scss" scoped>
.loading {
  @keyframes spin {
    to {
      transform: rotateZ(360deg);
    }
  }
  display: flex;
  height: 100%;
  width: 100%;
  justify-content: center;
  align-items: center;
  span {
    display: block;
    width: 60px;
    height: 60px;
    margin: 0 auto;
    border: 2px solid transparent;
    border-top-color: #142a5f;
    border-radius: 50%;
    animation: spin ease 1000ms infinite;
  }
}
.weather {
  transition: 500ms ease;
  overflow: scroll;
  width: 100%;
  height: 100%;
  overflow: hidden;
  .weather-wrap {
    overflow: hidden;
    max-width: 1024px;
    margin: 0 auto;
  }
}
</style>
