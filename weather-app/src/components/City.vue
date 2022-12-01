<!-- eslint-disable vue/multi-word-component-names -->
<template>
  <div class="city">
    <span>{{ city.name }}</span>
    <span>{{ city.main.temp }} &deg;C</span>
    <div class="weather">
      <img
        :src="`http://openweathermap.org/img/wn/${city.weather[0].icon}@2x.png`"
        alt=""
      />
      <div>{{ city.weather[0].description }}</div>
      <div>Усеща се като {{ city.main.feels_like }} &deg;C</div>
      <div>Влажност {{ city.main.humidity }}%</div>
      <div>Апн. налягане {{ city.main.pressure }} hPa</div>
    </div>
    <div class="video">
      <video
        autoplay
        loop
        muted
        :src="require(`../../public/videos/${city.weather[0].icon}.mp4`)"
      ></video>
      <div class="bg-overlay"></div>
    </div>
  </div>
</template>

<script>
export default {
  // eslint-disable-next-line vue/multi-word-component-names
  name: "city",
  props: ["city"],
};
</script>

<style lang="scss" scoped>
.city {
  display: flex;
  position: relative;
  flex-direction: column;
  padding: 20px;
  flex-basis: 50%;
  min-height: 250px;
  color: #fff;
  box-shadow: 0 1px 2px 0 rgba(0, 0, 0, 0.05);

  span {
    z-index: 1;
    text-transform: capitalize;
    display: block;
    font-size: 25px;
    font-weight: 600;
  }

  .weather {
    display: flex;
    z-index: 1;
    justify-content: flex-end;
    align-items: flex-end;
    flex: 1;
    flex-direction: column;

    span {
      font-size: 35px;
      margin-right: 8px;
    }

    img {
      height: 50px;
      width: auto;
    }
  }

  .video {
    overflow: hidden;
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
  }

  video {
    height: 100%;
    @media (min-width: 900px) {
      height: auto;
      width: 100%;
    }
  }

  .bg-overlay {
    position: absolute;
    height: 100%;
    width: 100%;
    top: 0;
    background-color: rgba(0, 0, 0, 0.2);
  }
}
</style>
