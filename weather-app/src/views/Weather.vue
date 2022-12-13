<!-- eslint-disable vue/multi-word-component-names -->
<template>
  <div class="main">
    <div v-if="loading" class="loading">
      <span></span>
    </div>
    <div v-else class="weather" :class="{ day: isDay, night: !isDay }">
      <div class="weather-wrap">
        <CurrentWeather :isDay="isDay" :currentWeather="currentWeather" />
        <HourlyWeather :forecast="forecast" />
        <transition name="bounce">
          <DailyForecast
            v-if="!detailsForecast"
            :forecast="forecast"
            @get-selected-day="getSelectedDay"
          />
          <DetailsDailyForecast
            v-else
            :selectedDayForecast="selectedDayForecast"
            @returnToDailyForecast="returnToDailyForecast"
          />
        </transition>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import CurrentWeather from "@/components/CurrentWeather.vue";
import HourlyWeather from "@/components/HourlyWeather.vue";
import DailyForecast from "@/components/DailyForecast.vue";
import DetailsDailyForecast from "@/components/DetailsDailyForecast.vue";

export default {
  // eslint-disable-next-line vue/multi-word-component-names
  name: "Weather",
  props: ["cities", "isDay"],
  components: {
    CurrentWeather,
    HourlyWeather,
    DailyForecast,
    DetailsDailyForecast,
  },
  data() {
    return {
      APIKey: process.env.VUE_APP_API_EXPLORER_API_KEY,
      detailsForecast: false,
      forecast: null,
      selectedDayForecast: null,
      currentWeather: null,
      loading: true,
    };
  },
  created() {
    this.getCurrentTime();
    this.getWeather();
  },
  methods: {
    returnToDailyForecast() {
      this.detailsForecast = false;
    },
    getSelectedDay(date) {
      this.selectedDayForecast = this.forecast.forecast.forecastday.find(
        (f) => f.date_epoch == date
      );
      this.detailsForecast = true;
    },
    getCurrentTime() {
      const dateObject = new Date();
      const currentTime = dateObject.getHours();

      const currentCity = this.cities.find(
        (c) => c.name == this.$route.params.city
      );

      if (currentCity == undefined) {
        this.$router.push("/");
        return;
      }

      this.currentWeather = currentCity;

      const sunrise = new Date(currentCity.sys.sunrise * 1000).getHours();
      const sunset = new Date(currentCity.sys.sunset * 1000).getHours();

      if (currentTime > sunrise && currentTime < sunset) {
        this.$emit("is-day", true);
      } else {
        this.$emit("is-day", false);
      }
    },
    getWeather() {
      const currentCity = this.cities.find(
        (c) => c.name == this.$route.params.city
      );

      if (currentCity == undefined) {
        return;
      }

      axios
        .get(
          `http://api.weatherapi.com/v1/forecast.json?key=${this.APIKey}&q=${currentCity.name}&days=7&lang=bg&aqi=yes&alerts=yes`
        )
        .then((responce) => {
          this.forecast = responce.data;
        })
        .then(() => {
          this.loading = false;
        });
    },
  },
};
</script>

<style lang="scss" scoped>
/* width */
::-webkit-scrollbar {
  width: 10px;
  height: 10px;
}

/* Track */
::-webkit-scrollbar-track {
  box-shadow: inset 0 0 5px grey;
  border-radius: 10px;
}

/* Handle */
::-webkit-scrollbar-thumb {
  background: rgb(7, 60, 131);
  border-radius: 10px;
}

/* Handle on hover */
::-webkit-scrollbar-thumb:hover {
  background: #5f61ee;
}

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
  overflow: auto;

  .weather-wrap {
    overflow: hidden;
    max-width: 1024px;
    margin: 0 auto;
  }

  .bounce-enter-active {
    animation: bounce-in 0.5s;
  }
  .bounce-leave-active {
    animation: bounce-in 0.5s reverse;
  }
  @keyframes bounce-in {
    0% {
      transform: scale(0);
    }
    50% {
      transform: scale(1.25);
    }
    100% {
      transform: scale(1);
    }
  }
}
</style>
