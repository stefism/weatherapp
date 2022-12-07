<template>
  <div class="current-weather">
    <div class="container">
      <div class="weather-info">
        <span class="city">{{ currentWeather.name }}</span>
        <span class="current-temperature">{{ currentWeather.main.temp }}&deg;</span>
        <div class="high-low-temperatures">
          <div class="high">
            макс.&nbsp; <i class="fas fa-chevron-up"></i>
            <span>{{ currentWeather.main.temp_max }}&deg;</span>
          </div>
          <div class="low">
            мин.&nbsp; <i class="fas fa-chevron-down"></i>
            <span>{{ currentWeather.main.temp_min }}&deg;</span>
          </div>
        </div>
        <span class="condition">{{
          currentWeather.weather[0].description
        }}</span>
        <span class="feels-like"
          >Усеща се като {{ currentWeather.main.feels_like }}&deg;</span
        >
      </div>
      <div class="weather-icon">
        <img v-if="isDay" src="../../public/sun.png" alt="sun" />
        <img v-else src="../../public/moon.png" alt="sun" />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "CurrentWeather",
  props: ["isDay", "currentWeather"],
};
</script>

<style lang="scss" scoped>
.current-weather {
  margin-top: 90px;
  border-bottom: 1px solid rgba(255, 255, 255, 0.7);
  .container {
    margin: 30px 0;
    min-height: 366px;
    position: relative;
    display: flex;
    color: #fff;
  }
  span {
    display: block;
  }
  .weather-info {
    padding-top: 30px;
    flex: 1;
    .city {
      font-size: 24px;
      font-weight: 500;
    }
    .current-temperature {
      margin-top: 40px;
      font-size: 80px;
      font-weight: 600;
    }
  }
  .high-low-temperatures {
    display: flex;
    gap: 16px;
    .high,
    .low {
      display: flex;
      span {
        margin-left: 8px;
      }
    }
  }
  .condition {
    text-transform: capitalize;
    font-size: 24px;
    font-weight: 500;
    margin: 60px 0 8px 0;
  }
  .feels-like {
    font-size: 14px;
    font-weight: 400;
  }
  .weather-icon {
    height: 100%;
    position: absolute;
    right: -180px;
    display: flex;
    align-items: center;
    img {
      width: 366px;
      height: 366px;
    }
  }
}
</style>