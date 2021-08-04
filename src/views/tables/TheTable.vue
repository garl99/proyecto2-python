<template>
  <v-card style="width: 100%; border-radius: 10px">
    <v-card-title>
      {{ title }}
      <v-spacer></v-spacer>
      <v-text-field
        v-if="!this.dropdown"
        v-model="search"
        append-icon="mdi-magnify"
        label="Buscar"
        single-line
        hide-details
      ></v-text-field>
      <v-select
        v-else
        :items="getItems()"
        v-model="ingredientSelected"
        label="Ingredientes"
      ></v-select>
    </v-card-title>
    <v-data-table
      :loading="loading"
      :headers="headers"
      :items="!this.dropdown ? data : dataFilterByIngredient"
      :search="search"
    ></v-data-table>
  </v-card>
</template>

<script>
export default {
  inject: ["ingredients"],
  props: ["data", "headers", "title", "dropdown", "loading"],
  data() {
    return {
      search: "",
      ingredientSelected: "Queso",
    };
  },
  computed: {
    dataFilterByIngredient() {
      return this.data[0].agrupado !== undefined
        ? this.data[0].agrupado[
            this.ingredientSelected.toLowerCase().replaceAll(" ", "_")
          ]
        : [];
    },
  },
  methods: {
    getItems() {
      let items = [];
      this.ingredients.map((ing) => {
        items.push(ing.name);
      });
      return items;
    },
  },
};
</script>

<style></style>
