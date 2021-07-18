<template>
  <v-app>
    <v-main>
      <div class="links__container">
        <a :class="{ active: currentOrder }" v-on:click="activeLink(true)"
          >Hacer pedido</a
        >
        <a :class="{ active: currentReport }" v-on:click="activeLink(false)"
          >Reportes</a
        >
      </div>
      <router-view />
    </v-main>
  </v-app>
</template>

<script>
export default {
  name: "App",

  data() {
    return {
      currentOrder: false,
      currentReport: false,
    };
  },
  methods: {
    activeLink(flag) {
      if (flag) {
        this.currentOrder = true;
        this.currentReport = false;
        this.$router.push("/orders");
      } else {
        this.currentReport = true;
        this.currentOrder = false;
        this.$router.push("/reports");
      }
    },
  },
};
</script>

<style scoped>
.links__container {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-top: 50px;
}
.links__container > a {
  margin: 0 10px;
  display: inline-block;
  padding: 5px 10px;
  text-decoration: none;
  cursor: pointer;
  background-color: #3992eb;
  color: white;
  border-radius: 80px;
}

.links__container > a:hover {
  background-color: #1976d2;
}

.active {
  background-color: #1976d2 !important;
}
</style>
