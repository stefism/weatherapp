<template>
  <div @click="returnToDailyForecast" style="cursor: pointer">
    <h4 class="header">
      Детайлна прогноза за
      {{
        new Date(selectedDayForecast.date_epoch * 1000).toLocaleString(
          "bg-bg",
          {
            weekday: "long",
          }
        )
      }}
    </h4>
    <div class="details-daily-forecast">
      <div class="left">
        <p>
          <v-icon>mdi-theme-light-dark</v-icon> осветеност на луната:
          <b>{{ selectedDayForecast.astro.moon_illumination }}</b
          >%
        </p>
        <p>
          <v-icon>mdi-moon-waning-crescent</v-icon> лунен изгрев:
          {{ selectedDayForecast.astro.moonrise }}
        </p>
        <p>
          <v-icon>mdi-moon-waxing-crescent</v-icon> лунен залез:
          {{ selectedDayForecast.astro.moonset }}
        </p>
        <p>
          <v-icon>mdi-weather-dust</v-icon> влажност:
          {{ selectedDayForecast.day.avghumidity }}%
        </p>
      </div>
      <div class="center">
        <p>
          <v-icon>mdi-sun-clock-outline</v-icon> изгрев:
          {{ selectedDayForecast.astro.sunrise }}
        </p>
        <p>
          <v-icon>mdi-sun-clock</v-icon> залез:
          {{ selectedDayForecast.astro.sunset }}
        </p>
        <p>
          <v-icon>mdi-weather-pouring</v-icon> шанс за дъжд:
          {{ selectedDayForecast.day.daily_chance_of_rain }}%
        </p>
        <p>
          <v-icon>mdi-weather-snowy-heavy</v-icon> шанс за сняг:
          {{ selectedDayForecast.day.daily_chance_of_snow }}%
        </p>
      </div>
      <div class="right">
        <p>
          <v-icon>mdi-thermometer-lines</v-icon> средна t&deg;:
          {{ selectedDayForecast.day.avgtemp_c }}&deg;
        </p>
        <p>
          <v-icon>mdi-thermometer-minus</v-icon> минимална t&deg;:
          {{ selectedDayForecast.day.mintemp_c }}&deg;
        </p>
        <p>
          <v-icon>mdi-thermometer-plus</v-icon> максимална t&deg;:
          {{ selectedDayForecast.day.maxtemp_c }}&deg;
        </p>
        <p>
          <v-icon>mdi-weather-tornado</v-icon> вятър:
          {{ selectedDayForecast.day.maxwind_kph }} км/ч.;
        </p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ["selectedDayForecast"],
  created() {
    console.log("selectedDayForecast", this.selectedDayForecast);
  },
  methods: {
    returnToDailyForecast() {
      this.$emit("returnToDailyForecast");
    },
  },
};
</script>

<style lang="scss" scoped>
.header {
  display: flex;
  color: white;
  justify-content: center;
  margin-bottom: 10px;
  margin-top: 10px;
}

.details-daily-forecast {
  color: white;
  display: flex;
  justify-content: space-around;
  text-transform: lowercase;
  margin-bottom: 10px;

  .v-icon {
    color: white;
    text-shadow: 2px 2px 3px #040404, 2px 2px 3px #040404;
  }

  .left {
    display: flex;
    flex-direction: column;
    justify-content: left;
  }

  .center {
    display: flex;
    flex-direction: column;
    justify-content: left;
    align-items: flex-start;
  }
}
</style>
