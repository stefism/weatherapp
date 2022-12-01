<template>
  <div class="main">
    <Navigation class="navigation" />
    <router-view />
  </div>
</template>

<script>
import axios from "axios";
import Navigation from "@/components/Navigation.vue";

export default {
  name: "App",
  components: { Navigation },
  data() {
    return {
      APIKey: process.env.VUE_APP_OPENWEATHER_API_KEY,
      city: "Sofia",
    };
  },
  created() {
    this.getCurrentWeather();
  },

  methods: {
    getCurrentWeather() {
      axios
        .get(
          `https://api.openweathermap.org/data/2.5/weather?q=${
            this.city
          },${"BG"}&units=metric&appid=${this.APIKey}`
        )
        .then((responce) => {
          console.log(responce);
        });
    },
  },
};
</script>

<style lang="scss">
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: "Quicksand", sans-serif;
}

.main {
  height: 100vh;
  .navigation {
    z-index: 99;
    position: fixed;
    max-width: 1024px;
    width: 100%;
    box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1),
      0 2px 4px -1px rgba(0, 0, 0, 0.06);
  }
}
</style>
