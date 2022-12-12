<template>
  <div class="hourly-weather">
    <div class="container">
      <div class="hourly-temp">
        <div
          class="hour"
          v-for="(hour, index) in currentDayHourlyForecast"
          :key="index"
        >
          <span>{{ hour.time.slice(11) }}</span>
          <span>
            <img :src="hour.condition.icon" alt="hourly icon" />
          </span>
          <span>{{ hour.temp_c }}&deg;</span>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "HourlyWeather",
  props: ["forecast"],
  created() {
    this.currentDayHourlyForecast = this.forecast.forecast.forecastday[0].hour;
  },
  data() {
    return {
      currentDayHourlyForecast: [],
    };
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
  background: rgb(72, 138, 224);
  border-radius: 10px;
}

/* Handle on hover */
::-webkit-scrollbar-thumb:hover {
  background: #5f61ee;
}

.hourly-weather {
  padding: 0px 0;
  border-bottom: 1px solid rgba(255, 255, 255, 0.7);

  .container {
    display: flex;
    max-width: 100vw;
    overflow: auto;
    padding: 10px;
    margin: 20px 20px;
  }
}

.hourly-temp {
  display: flex;

  .hour {
    color: #fff;
    display: flex;
    flex-direction: column;
    text-align: center;
    min-width: 45px;
    margin-right: 30px;

    img {
      width: auto;
      height: 45px;
      display: block;
    }

    span {
      display: block;
      align-self: center;
    }

    span:nth-child(1) {
      font-weight: 300;
      margin-bottom: 12px;
    }

    span:nth-child(2) {
      margin-bottom: 12px;
    }

    span:nth-child(3) {
      font-weight: 500;
    }
  }
}
</style>
