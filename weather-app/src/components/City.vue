<!-- eslint-disable vue/multi-word-component-names -->
<template>
  <div @click="goToWeather" class="city">
    <i
      v-if="isInEditMode"
      @click="removeCity(city.programId)"
      class="far fa-trash-alt edit"
      ref="edit"
    ></i>
    <span>{{ city.name }} - {{ city.sys.country }}</span>
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
  props: ["city", "isInEditMode"],
  methods: {
    removeCity(cityId) {
      this.$emit("remove-city", cityId);
    },
    goToWeather(e) {
      if (e.target != this.$refs.edit) {
        this.$router.push({
          name: "Weather",
          params: { city: this.city.name },
        });
      }
    },
  },
};
</script>

<style lang="scss" scoped>
.text {
   box-shadow: 1px 1px 2px 2px rgba(0, 0, 0, 0.2);
}

.city {
  display: flex;
  position: relative;
  flex-direction: column;
  padding: 20px;
  flex-basis: 50%;
  min-height: 250px;
  color: #fff;
  box-shadow: 0 1px 2px 0 rgba(0, 0, 0, 0.05);

  .edit {
    border-radius: 0px 15px 0 0;
    border: 10px solid rgb(77, 77, 77);
    background-color: rgb(77, 77, 77);
    z-index: 1;
    font-size: 20px;
    position: absolute;
    bottom: 0px;
    left: 0px;
  }

  span {
    z-index: 1;
    text-transform: capitalize;
    display: block;
    font-size: 25px;
    font-weight: 600;
    text-shadow: 3px 3px #040404;
  }

  .weather {
    display: flex;
    z-index: 1;
    justify-content: flex-end;
    align-items: flex-end;
    flex: 1;
    flex-direction: column;
    text-shadow: 2px 2px 3px #040404, 2px 2px 3px #040404;

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
