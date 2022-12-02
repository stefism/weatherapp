<template>
  <div class="main">
    <Modal v-if="modalOpen" @close-modal="toggleModal" />
    <Navigation @add-city="toggleModal" />
    <router-view :cities="citiesWeather" />
  </div>
</template>

<script>
import axios from "axios";
import Navigation from "@/components/Navigation.vue";
import Modal from "@/components/Modal.vue";

export default {
  name: "App",
  components: { Navigation, Modal },
  data() {
    return {
      modalOpen: null,
      APIKey: process.env.VUE_APP_OPENWEATHER_API_KEY,
      cities: ["Sofia", "Varna", "Bourgas"],
      citiesWeather: [],
    };
  },
  created() {
    this.getCurrentWeather();
  },

  methods: {
    toggleModal() {
      this.modalOpen = !this.modalOpen;
    },
    getCurrentWeather() {
      this.cities.forEach((city) => {
        axios
          .get(
            `https://api.openweathermap.org/data/2.5/weather?q=${city},${"BG"}&lang=bg&units=metric&appid=${
              this.APIKey
            }`
          )
          .then((responce) => {
            this.citiesWeather.push(responce.data);
          });
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
  max-width: 1024px;
  margin: 0 auto;
  height: 100vh;

  .container {
    padding: 0 20px;
  }
}
</style>
