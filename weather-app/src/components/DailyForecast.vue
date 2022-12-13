<template>
  <div class="weekly-forecast">
    <div class="container">
      <div
        class="daily-forecast"
        v-for="(day, index) in dailyForecast"
        :key="index"
        @click="getSelectedDay(day.date_epoch)"
      >
        <div>
          <span class="week-day">
            {{
              new Date(day.date_epoch * 1000).toLocaleString("bg-bg", {
                weekday: "long",
              })
            }}
            -
            {{
              new Date(day.date_epoch * 1000).toLocaleString("bg-bg", {
                day: "2-digit",
              })
            }}
          </span>
        </div>

        <div class="condition">
          <img :src="day.day.condition.icon" alt="daily icon" />
          <span>{{ day.day.condition.text }}</span>
        </div>

        <div class="weather">
          <span class="high"
            ><v-icon>mdi-arrow-collapse-up</v-icon
            >{{ day.day.maxtemp_c }}&deg;</span
          >&nbsp;&nbsp;
          <span class="low"
            ><v-icon>mdi-arrow-collapse-down</v-icon
            >{{ day.day.mintemp_c }}&deg;</span
          >
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "DailyWeather",
  props: ["forecast"],
  data() {
    return {
      dailyForecast: this.forecast.forecast.forecastday.slice(1),
    };
  },
  created() {},
  methods: {
    getSelectedDay(date) {
      this.$emit("get-selected-day", date);
    },
  },
};
</script>

<style lang="scss" scoped>
.daily-forecast {
  display: flex;
  align-items: center;
  justify-content: space-between;
  color: #fff;
  cursor: pointer;

  .week-day {
    text-transform: capitalize;
  }

  div {
    flex: 1;
  }

  .condition {
    display: flex;
    align-items: center;
    justify-content: center;

    img {
      width: 45px;
    }
  }

  .weather {
    display: flex;
    justify-content: flex-end;

    span {
      min-width: 20px;
    }

    .high {
      font-weight: 500;
    }
  }
}

.daily-forecast:hover {
  background-color: rgb(153, 210, 248);
}
</style>
