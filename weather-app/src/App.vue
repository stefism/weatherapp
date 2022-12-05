<template>
  <div class="main">
    <Modal
      v-if="modalOpen"
      @close-modal="toggleModal"
      @addNewCityToWeather="addNewCityToWeather"
    />
    <Navigation
      :addCityActive="addCityActive"
      @add-city="toggleModal"
      @edit-city="toggleEditMode"
    />
    <router-view
      :cities="citiesWeather"
      :isInEditMode="isInEditMode"
      @remove-city="removeCity"
    />
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
      addCityActive: null,
      APIKey: process.env.VUE_APP_OPENWEATHER_API_KEY,
      cities: [
        { city: "Sofia", country: "BG", id: "1" },
        { city: "Varna", country: "BG", id: "2" },
        { city: "Bourgas", country: "BG", id: "3" },
      ],
      citiesWeather: [],
      selectedCountry: null,
    };
  },
  created() {
    this.getCurrentWeather();
  },
  watch: {
    $route() {
      this.checkRoute();
    },
  },
  methods: {
    addNewCityToWeather(countryCode, cityName) {
      const random = Math.random().toString(16).slice(2);
      this.cities.push({ city: cityName, country: countryCode, id: random });
      this.getCurrentWeather();
      this.modalOpen = false;
    },
    removeCity(cityId) {
      this.cities = this.cities.filter((c) => c.id != cityId);
      this.isInEditMode = false;
      this.getCurrentWeather();
    },
    checkRoute() {
      if (this.$route.name == "AddCity") {
        this.addCityActive = true;
      } else {
        this.addCityActive = false;
      }
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
            responce.data.programId = city.id;
            this.citiesWeather.push(responce.data);
          });
      });

      console.log(this.citiesWeather);
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
