<template>
  <div class="main">
    <Modal
      v-if="modalOpen"
      @close-modal="toggleModal"
      @addNewCityToWeather="addNewCityToWeather"
    />
    <Navigation @add-city="toggleModal" @edit-city="toggleEditMode" />
    <router-view :cities="citiesWeather" :isInEditMode="isInEditMode" />
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
      isInEditMode: null,
      modalOpen: null,
      APIKey: process.env.VUE_APP_OPENWEATHER_API_KEY,
      cities: [
        { city: "Sofia", country: "BG" },
        { city: "Varna", country: "BG" },
        { city: "Bourgas", country: "BG" },
      ],
      citiesWeather: [],
      selectedCountry: null,
    };
  },
  created() {
    this.getCurrentWeather();
  },

  methods: {
    addNewCityToWeather(countryCode, cityName) {
      this.cities.push({ city: cityName, country: countryCode });
      this.getCurrentWeather();
      this.modalOpen = false;
    },
    toggleModal() {
      this.modalOpen = !this.modalOpen;
    },
    toggleEditMode() {
      this.isInEditMode = !this.isInEditMode;
    },
    getCurrentWeather() {
      this.citiesWeather = [];
      this.cities.forEach((city) => {
        axios
          .get(
            `https://api.openweathermap.org/data/2.5/weather?q=${city.city},${city.country}&lang=bg&units=metric&appid=${this.APIKey}`
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
