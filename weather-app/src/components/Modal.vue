<!-- eslint-disable vue/multi-word-component-names -->
<template>
  <div @click="closeModal" class="modal" ref="modal">
    <div class="modal-wrap" ref="modalWrap">
      <!-- <v-autocomplete
        style="background-color: white"
        :items="countries"
        item-text="name"
      >
      </v-autocomplete> -->
      <label for="country-name">Изберете държава:</label>
      <input
        list="countries"
        type="text"
        name="country-name"
        placeholder="Търсете по име на държава"
        v-model="currentCountryName"
      />
      <datalist id="countries">
        <option
          v-for="(country, index) in countries"
          :key="index"
          :value="country.name"
        ></option>
      </datalist>
      <label for="city-name">Изберете град:</label>
      <input
        list="current-cities"
        type="text"
        name="city-name"
        placeholder="Търсете по име на град"
        v-model="currentCity"
      />
      <datalist id="current-cities">
        <option
          v-for="(city, index) in currentCities"
          :key="index"
          :value="city.name"
        ></option>
      </datalist>

      <v-btn @click="clearSelection">Изчисти</v-btn>
      <v-btn @click="addNewCityToWeather">Добави</v-btn>
    </div>
  </div>
</template>

<script>
import counrtyCodes from "../assets/countryCodes.json";
import cities from "../assets/cities.json";

export default {
  // eslint-disable-next-line vue/multi-word-component-names
  name: "modal",
  data() {
    return {
      countries: counrtyCodes,
      currentCountryName: null,
      currentCity: null,
    };
  },
  mounted() {},
  computed: {
    currentCities() {
      const countryObject = this.countries.find(
        (c) => c.name == this.currentCountryName
      );
      return (
        countryObject && cities.filter((c) => c.country == countryObject.code)
      );
    },
  },
  methods: {
    clearSelection() {
      this.currentCountryName = null;
      this.currentCity = null;
    },
    addNewCityToWeather() {
      const countryObject = this.countries.find(
        (c) => c.name == this.currentCountryName
      );
      this.$emit("addNewCityToWeather", countryObject.code, this.currentCity);
    },
    closeModal(e) {
      if (e.target == this.$refs.modal) {
        this.$emit("close-modal");
      }
    },
  },
};
</script>

<style lang="scss" scoped>
.modal {
  z-index: 101;
  background: rgba(0, 0, 0, 0.5);
  position: fixed;
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  label {
    color: #fff;
  }
  .modal-wrap {
    max-width: 500px;
    border-radius: 8px;
    width: 80%;
    padding: 20px;
    background-color: #31363d;
    box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1),
      0 2px 4px -1px rgba(0, 0, 0, 0.06);
    input {
      color: #fff;
      border: none;
      background-color: transparent;
      border-bottom: 1px solid #fff;
      padding: 6px 4px;
      margin: 10px 0 20px;
      width: 100%;
      &:focus {
        outline: none;
      }
    }
    button {
      background-color: #222325;
      color: #fff;
      padding: 6px 20px;
      border-radius: 8px;
      border: none;
      box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1),
        0 2px 4px -1px rgba(0, 0, 0, 0.06);
    }
  }
}
</style>
