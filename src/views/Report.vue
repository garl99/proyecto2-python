<template>
  <div class="about">
    <v-container>
      <v-row class="text-center">
        <v-col
          class="mb-5 mt-5"
          cols="12"
          style="display: flex; justify-content: center"
        >
          <v-card style="width: 80%">
            <v-toolbar
              color="blue darken-1"
              dark
              style="
                display: flex;
                justify-content: center;
                align-items: center;
                flex-direction: column;
              "
            >
              <v-toolbar-title>Ventas</v-toolbar-title>
              <template v-slot:extension>
                <v-tabs v-model="tabs" centered>
                  <v-tab
                    v-for="item in items"
                    :key="item"
                    :href="'#tab-' + item"
                  >
                    {{ item }}
                  </v-tab>
                </v-tabs>
              </template>
            </v-toolbar>

            <v-tabs-items v-model="tabs">
              <v-tab-item :value="'tab-General'">
                <the-table
                  :general="sandwiches"
                  :headers="headers"
                  title="General"
                ></the-table
              ></v-tab-item>
              <v-tab-item :value="'tab-x Día'">
                <the-table
                  :general="sandwiches"
                  :headers="headers"
                  title="Por día"
                ></the-table>
              </v-tab-item>
              <v-tab-item :value="'tab-x Tamaño de Sand.'">
                <the-table
                  :general="sandwiches"
                  :headers="headers"
                  title="Por tamaño de sandwich"
                ></the-table>
              </v-tab-item>
              <v-tab-item :value="'tab-x Ingredientes'">
                <the-table
                  :general="sandwiches"
                  :headers="headers"
                  title="Por ingredientes"
                ></the-table>
              </v-tab-item>
              <v-tab-item :value="'tab-x Clientes'">
                <the-table
                  :general="sandwiches"
                  :headers="headers"
                  title="Por clientes"
                ></the-table>
              </v-tab-item>
            </v-tabs-items>
          </v-card>
        </v-col>
      </v-row>
    </v-container>
  </div>
</template>

<script>
import TheTable from "./tables/TheTable.vue";
export default {
  components: {
    TheTable,
  },
  created() {
    console.log(this.sandwiches);
  },
  data: () => ({
    items: [
      "General",
      "x Día",
      "x Tamaño de Sand.",
      "x Ingredientes",
      "x Clientes",
    ],
    tabs: "tab-General",
    sandwiches: [
      {
        id: 12345,
        customer: "Gabriel Romero",
        size: "triple",
        ingredients: "queso,pepperoni,jamon",
        total: 400,
        date: "21/04/2021",
      },
      {
        id: 12346,
        customer: "Daniel Diaz",
        size: "doble",
        ingredients: "queso,pepperoni,jamon",
        total: 400,
        date: "20/07/2021",
      },
      {
        id: 12347,
        customer: "Ricardo Perez",
        size: "triple",
        ingredients: "queso,pepperoni,jamon",
        total: 400,
        date: "20/06/2021",
      },
      {
        id: 12348,
        customer: "Michael Amariscua",
        size: "individual",
        ingredients: "queso,pepperoni,jamon",
        total: 400,
        date: "21/05/2021",
      },
      {
        id: 12349,
        customer: "Gabriel Romero",
        size: "triple",
        ingredients: "queso,pepperoni,jamon",
        total: 400,
        date: "21/03/2021",
      },
      {
        id: 12350,
        customer: "Daniel Diaz",
        size: "doble",
        ingredients: "queso,pepperoni,jamon",
        total: 400,
        date: "20/07/2021",
      },
      {
        id: 12351,
        customer: "Ricardo Perez",
        size: "triple",
        ingredients: "queso,pepperoni,jamon",
        total: 400,
        date: "10/03/2021",
      },
      {
        id: 12352,
        customer: "Michael Amariscua",
        size: "individual",
        ingredients: "queso,pepperoni,jamon",
        total: 400,
        date: "15/02/2021",
      },
    ],
    headers: [
      {
        text: "ID Venta",
        align: "start",
        value: "id",
      },
      { text: "Cliente", value: "customer" },
      { text: "Tamaño", value: "size" },
      { text: "Ingredientes", value: "ingredients" },
      { text: "Total", value: "total" },
      { text: "Fecha", value: "date" },
    ],
  }),
};
</script>
