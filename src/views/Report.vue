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
                  :data="reports.General"
                  :headers="headers"
                  :dropdown="false"
                  title="General"
                ></the-table
              ></v-tab-item>
              <v-tab-item :value="'tab-x Día'">
                <the-table
                  :data="reports.Day"
                  :headers="headers"
                  :dropdown="false"
                  title="Por día"
                ></the-table>
              </v-tab-item>
              <v-tab-item :value="'tab-x Tamaño de Sand.'">
                <the-table
                  :data="reports.Size"
                  :headers="headers"
                  :dropdown="false"
                  title="Por tamaño de sandwich"
                ></the-table>
              </v-tab-item>
              <v-tab-item :value="'tab-x Ingredientes'">
                <the-table
                  :data="reports.Ingredients"
                  :headers="headers"
                  :dropdown="true"
                  title="Por ingredientes"
                ></the-table>
              </v-tab-item>
              <v-tab-item :value="'tab-x Clientes'">
                <the-table
                  :data="reports.Clients"
                  :headers="headers"
                  :dropdown="false"
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
//import reports from "../data/pedidos.js";

export default {
  components: {
    TheTable,
  },
  created() {
    this.getReports();
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
    reports: {},
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
  methods: {
    getReports() {
      /*setTimeout(() => {
        let reportsObject = reports[0];
        this.reports = { ...reportsObject };
      }, 5000);*/
      var url = "http://190.203.203.128:5000/pedidos/";
      this.$axios
        .get(url)
        .then((response) => {
          let reportsObject = response.data.result[0];
          this.reports = { ...reportsObject };
        })
        .catch((error) => {
          console.log(error);
        });
    },
  },
};
</script>
