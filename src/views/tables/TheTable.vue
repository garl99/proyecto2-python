<template>
  <v-card style="width: 100%; border-radius: 10px">
    <v-card-title>
      {{ title }}
      <v-spacer></v-spacer>
      <v-text-field
        v-model="search"
        append-icon="mdi-magnify"
        label="Buscar"
        single-line
        hide-details
      ></v-text-field>
    </v-card-title>
    <v-data-table
      :headers="headers"
      :items="general"
      :search="search"
    ></v-data-table>
  </v-card>
</template>

<script>
export default {
  props: ["general", "headers", "title"],
  data() {
    return {
      search: "",
    };
  },
};
</script>

<style></style>
