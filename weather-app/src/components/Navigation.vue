<!-- eslint-disable vue/multi-word-component-names -->
<template>
  <div>
    <header v-if="addCityActive" class="container add-city">
      <nav>
        <span>Add City</span>
        <div class="right">
          <i @click="$emit('edit-city')" class="far fa-edit"></i>
          <i @click="reloadApp" class="fas fa-sync"></i>
          <i @click="$emit('add-city')" class="fas fa-plus"></i>
        </div>
      </nav>
    </header>
    <header v-else class="container">
      <nav>
        <router-link class="router-link" :to="{ name: 'AddCity' }">
          <i class="fas fa-plus"></i>
        </router-link>
        <span
          >{{ new Date().toLocaleString("bg-bg", { weekday: "long" }) }}
          {{ new Date().toLocaleString("bg-bg", { month: "short" }) }}
          {{ new Date().toLocaleString("bg-bg", { day: "2-digit" }) }}</span
        >
        <span>&deg; C</span>
      </nav>
    </header>
  </div>
</template>

<script>
export default {
  // eslint-disable-next-line vue/multi-word-component-names
  name: "navigation",
  props: ["addCityActive"],
  methods: {
    reloadApp() {
      location.reload();
    },
  },
};
</script>

<style lang="scss" scoped>
.add-city {
  background-color: #313640;
}

header {
  z-index: 99;
  position: fixed;
  max-width: 1024px;
  width: 100%;
  box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1),
    0 2px 4px -1px rgba(0, 0, 0, 0.06);
  nav {
    display: flex;
    color: #fff;
    padding: 30px 0;
    justify-content: space-between;
  }

  .router-link {
    color: #fff;
  }

  .right {
    i {
      font-size: 20px;
    }

    i:hover {
      cursor: pointer;
      color: red;
    }

    i:nth-child(2),
    i:nth-child(3) {
      margin-left: 16px;
    }

    i:nth-child(3) {
      margin-right: 20px;
    }
  }

  span {
    font-weight: 600;
    margin-left: 20px;
  }
}
</style>
