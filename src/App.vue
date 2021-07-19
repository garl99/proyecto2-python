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
      sizes: [
        {
          id: 1,
          name: "Triple",
          price: 580,
        },
        {
          id: 2,
          name: "Doble",
          price: 430,
        },
        {
          id: 3,
          name: "Individual",
          price: 280,
        },
      ],
      ingredients: [
        {
          id: 1,
          name: "Jamon",
          price: 40,
        },
        {
          id: 2,
          name: "Champiñones",
          price: 30,
        },
        {
          id: 3,
          name: "Pimenton",
          price: 35,
        },
        {
          id: 4,
          name: "Doble Queso",
          price: 40,
        },
        {
          id: 5,
          name: "Aceitunas",
          price: 57.5,
        },
        {
          id: 6,
          name: "Pepperoni",
          price: 38.5,
        },
        {
          id: 7,
          name: "Salchichon",
          price: 62.5,
        },
      ],
    };
  },
  provide() {
    return {
      sizes: this.sizes,
      ingredients: this.ingredients,
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
