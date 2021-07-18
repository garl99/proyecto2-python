<template>
  <div class="about">
    <add-sandwich></add-sandwich>
    <v-container>
      <v-row class="mt-5">
        <v-col class="mb-5 mt-5" cols="12">
          <h1 class="text-center">Pedido</h1>
          <v-btn
            style="margin-left: 20px"
            color="green"
            small
            rounded
            dark
            @click="openModal()"
          >
            Agregar
          </v-btn>
          <div
            class="mb-5 mt-5"
            style="display: flex; justify-content: center; flex-wrap: wrap"
          >
            <v-card
              v-for="item in order"
              :key="item.id"
              class="mx-5 mb-5"
              max-width="344"
              style="border-radius: 10px"
              outlined
            >
              <v-list-item three-line>
                <v-list-item-content>
                  <div class="text-overline mb-4">#{{ item.id }}</div>
                  <v-list-item-title class="text-h5 mb-1">
                    Sandwich {{ item.size }}
                  </v-list-item-title>
                  <v-list-item-subtitle
                    >Greyhound divisely hello coldly
                    fonwderfully</v-list-item-subtitle
                  >
                </v-list-item-content>

                <v-list-item-avatar size="80">
                  <v-img :src="link(item.size)"></v-img>
                </v-list-item-avatar>
              </v-list-item>

              <v-card-actions>
                <v-btn outlined rounded small> Eliminar</v-btn>
              </v-card-actions>
            </v-card>
          </div>
        </v-col>
      </v-row>
    </v-container>
  </div>
</template>

<script>
import { EventBus } from "@/event-bus";
import AddSandwich from "../components/AddSandwich.vue";

export default {
  components: {
    AddSandwich,
  },
  data() {
    return {
      order: [
        {
          id: 1,
          size: "Triple",
          ingredientes: [
            { name: "champiñon", value: 100 },
            { name: "queso", value: 100 },
          ],
          subtotal: 1200,
        },
        {
          id: 2,
          size: "Doble",
          ingredientes: [
            { name: "champiñon", value: 100 },
            { name: "queso", value: 100 },
          ],
          subtotal: 1200,
        },
        {
          id: 3,
          size: "Triple",
          ingredientes: [
            { name: "champiñon", value: 100 },
            { name: "queso", value: 100 },
          ],
          subtotal: 1200,
        },
        {
          id: 4,
          size: "Individual",
          ingredientes: [
            { name: "champiñon", value: 100 },
            { name: "queso", value: 100 },
          ],
          subtotal: 1200,
        },
      ],
    };
  },
  methods: {
    link(size) {
      if (size === "Triple") {
        return "https://static.vix.com/es/sites/default/files/imj/elgranchef/5/5597134000_d3c91a42e6_z.jpg";
      } else if (size === "Doble") {
        return "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcR4XJG9yfRrg0LHR1TyMzUyfSNCmZBSSO_j6DBCCUHil1jaMFXRZ1L5QBhqqnmSp-qaeXI&usqp=CAU";
      } else if (size === "Individual") {
        return "https://previews.123rf.com/images/gospix/gospix1705/gospix170500160/77885465-s%C3%A1ndwich-individual.jpg";
      }
    },
    openModal() {
      EventBus.$emit("open-add-sandwich");
    },
  },
};
</script>
