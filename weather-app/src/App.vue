<template>
  <div class="main">
    <Modal
      v-if="modalOpen"
      @close-modal="toggleModal"
      @addNewCityToWeather="addNewCityToWeather"
    />

    <Navigation
      :addCityActive="addCityActive"
      :isDay="isDay"
      @add-city="toggleModal"
      @edit-city="toggleEditMode"
    />

    <div v-if="!agreeCookies" class="consent">
      <p>
        Щеш - нещеш - бисквита ще ядеш. Ползувам бисквита, торта, паста и други
        за да ти дращя по локал сториджа. Там записвам кои градове ползваш,
        както и дали си съгласен с това.
      </p>
      <v-btn @click="acceptCookies">Ми убуу</v-btn>
    </div>

    <router-view
      :isDay="isDay"
      :cities="citiesWeather"
      :isInEditMode="isInEditMode"
      @remove-city="removeCity"
      @is-day="checkIsDayOrNight"
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
      isDay: null,
      isInEditMode: null,
      modalOpen: null,
      addCityActive: true,
      APIKey: process.env.VUE_APP_OPENWEATHER_API_KEY,
      cities: [],
      citiesWeather: [],
      selectedCountry: null,
      agreeCookies: false,
    };
  },
  created() {
    this.verifyCookieConsent();
    this.manageLocalStorageCities();
    this.getCurrentWeather();
  },
  watch: {
    $route() {
      this.checkRoute();
    },
  },
  methods: {
    verifyCookieConsent() {
      const cookie = window.localStorage.getItem("weatherAppAgreeCookie");
      if (cookie) {
        this.agreeCookies = true;
      }
    },
    acceptCookies() {
      window.localStorage.setItem("weatherAppAgreeCookie", "true");
      this.agreeCookies = true;
    },
    manageLocalStorageCities() {
      const initialDefaultCities = [
        { city: "Sofia", country: "BG", id: "1" },
        { city: "Varna", country: "BG", id: "2" },
        { city: "Bourgas", country: "BG", id: "3" },
      ];

      const cities = window.localStorage.getItem("weatherAppCities");
      const citiesObj = JSON.parse(cities);

      if (citiesObj == null) {
        window.localStorage.setItem(
          "weatherAppCities",
          JSON.stringify(this.initialDefaultCities)
        );

        this.cities = initialDefaultCities;
      } else {
        this.cities = citiesObj;
      }
    },
    checkIsDayOrNight(isDay) {
      this.isDay = isDay;
    },
    addNewCityToWeather(countryCode, cityName) {
      const random = Math.random().toString(16).slice(2);
      this.cities.push({ city: cityName, country: countryCode, id: random });

      window.localStorage.setItem(
        "weatherAppCities",
        JSON.stringify(this.cities)
      );

      this.getCurrentWeather();
      this.modalOpen = false;
    },
    removeCity(cityId) {
      this.cities = this.cities.filter((c) => c.id != cityId);

      window.localStorage.setItem(
        "weatherAppCities",
        JSON.stringify(this.cities)
      );

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

.consent {
  background-color: green;
  margin-top: 44px;
  display: flex;
  z-index: 99;
  position: absolute;
  color: white;
}

.day {
  transition: 500ms ease all;
  background-color: rgb(59, 150, 249);
}

.night {
  transition: 500ms ease all;
  background-color: rgb(20, 42, 95);
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
