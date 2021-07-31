<template>
  <v-row justify="center" style="overflow: hidden">
    <v-dialog v-model="dialog" persistent max-width="700px" scrollable>
      <v-card>
        <v-card-title style="background-color: green">
          <span class="text-h5" style="color: #fff; font-weight: bold"
            >Agregar Sandwiches</span
          >
        </v-card-title>
        <v-card-text>
          <v-container>
            <v-row class="mb-5">
              <v-col cols="12">
                <p style="margin-top: 5px; font-size: 16px; color: #000">
                  Tamaño
                </p>
                <v-radio-group v-model="size" row>
                  <v-radio
                    v-for="size in sizesDialog"
                    :key="size.id"
                    :label="size.name"
                    :value="size.id"
                  ></v-radio>
                </v-radio-group>
              </v-col>
              <v-col cols="12">
                <p style="margin-top: 5px; font-size: 16px; color: #000">
                  Ingredientes
                </p>
                <div style="display: flex; flex-wrap: wrap">
                  <v-checkbox
                    style="width: 30%"
                    v-for="ing in ingredientsDialog"
                    :key="ing.id"
                    v-model="ing.added"
                    :label="ing.name"
                    color="green"
                    :value="ing.id"
                    hide-details
                  ></v-checkbox>
                </div>
              </v-col>
              <v-col
                cols="12"
                class="mt-5"
                style="display: flex; justify-content: space-between"
              >
                <v-btn
                  color="primary"
                  elevation="2"
                  outlined
                  rounded
                  small
                  :disabled="size !== null ? false : true"
                  @click="addSandwichToList()"
                >
                  Agregar sandwich
                </v-btn>
                <div style="display: flex; flex-direction: column">
                  <p style="margin: 0; font-size: 16px; color: #000">
                    Subtotal
                  </p>
                  <span>$ {{ getSubtotal }}</span>
                </div>
              </v-col>
            </v-row>
            <v-divider v-if="sandwichesAdded.length !== 0"></v-divider>
            <v-row class="my-5" v-if="sandwichesAdded.length !== 0">
              <v-col cols="12">
                <v-list>
                  <v-subheader>Sandwiches agregados</v-subheader>
                  <v-list-item v-for="sand in sandwichesAdded" :key="sand.id">
                    <v-list-item-avatar size="50">
                      <v-img :src="link(sand.sizeLabel)"></v-img>
                    </v-list-item-avatar>

                    <v-list-item-content>
                      <v-list-item-title
                        v-text="
                          'Sandwich ' + sand.sizeLabel + ' - $' + sand.subtotal
                        "
                      ></v-list-item-title>

                      <v-list-item-subtitle
                        v-text="
                          'Ingredientes: ' +
                          arrayToString(sand.ingredientsLabels)
                        "
                      ></v-list-item-subtitle>
                    </v-list-item-content>
                    <v-list-item-action>
                      <v-btn icon @click="remove(sand.id)">
                        <v-icon color="red lighten-1">mdi-delete</v-icon>
                      </v-btn>
                    </v-list-item-action>
                  </v-list-item>
                </v-list>
              </v-col>
            </v-row>
            <v-divider v-if="sandwichesAdded.length !== 0"></v-divider>
            <v-row class="mt-5" v-if="sandwichesAdded.length !== 0">
              <v-col cols="12" style="display: flex; justify-content: flex-end">
                <div style="display: flex; flex-direction: column">
                  <p style="margin: 0; font-size: 16px; color: #000">Total</p>
                  <span>$ {{ getTotal }}</span>
                </div>
              </v-col>
            </v-row>
          </v-container>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn
            color="red"
            elevation="2"
            outlined
            rounded
            small
            @click="dialog = false"
          >
            Cerrar
          </v-btn>
          <v-btn
            v-if="sandwichesAdded.length !== 0"
            color="green"
            elevation="2"
            outlined
            rounded
            small
            @click="confirm()"
          >
            Confirmar
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-row>
</template>

<script>
import { EventBus } from "@/event-bus";
export default {
  inject: ["sizes", "ingredients"],
  created() {
    EventBus.$on("open-add-sandwich", (total) => {
      this.total = total;
      this.initDialog();
    });
  },
  computed: {
    getSubtotal() {
      let subtotal = 0;
      let priceSize = 0;
      this.ingredientsDialog.forEach((element) => {
        if (element.added) {
          subtotal += subtotal + element.price;
        }
      });
      if (this.size) {
        let size = this.sizes.find((item) => item.id === this.size);
        priceSize = size.price;
      }
      return subtotal + priceSize;
    },
    getTotal() {
      let total = this.total;
      this.sandwichesAdded.forEach((element) => {
        total += element.subtotal;
      });
      return total;
    },
  },
  data() {
    return {
      dialog: false,
      size: null,
      sizesDialog: [],
      ingredientsDialog: [],
      sandwichesAdded: [],
      cheese: null,
      total: 0,
    };
  },
  methods: {
    initDialog() {
      this.dialog = true;
      this.size = null;
      this.sizesDialog = this.sizes;
      this.sandwichesAdded = [];
      this.initIngredients();
    },
    initIngredients() {
      this.ingredientsDialog = [];
      this.ingredients.map((ing) => {
        if (ing.id != 8) {
          let item = {
            id: ing.id,
            name: ing.name,
            price: ing.price,
            added: false,
          };
          this.ingredientsDialog.push(item);
        }
      });
      this.cheese = this.ingredients[7];
    },
    addSandwichToList() {
      let sandwich = {
        id: Math.round(Math.random() * (99999999 - 1) + 1),
        size: this.size,
        sizeLabel: this.sizes.find((item) => item.id === this.size).name,
        ingredients: this.getIngredients(true),
        ingredientsLabels: this.getIngredients(false),
        subtotal: this.getSubtotal,
      };
      this.sandwichesAdded.push(sandwich);
      console.log(this.sandwichesAdded);
      this.initIngredients();

      this.size = null;
    },
    getIngredients(flag) {
      let ingredients = [];
      this.ingredientsDialog.forEach((element) => {
        if (element.added) {
          flag === true
            ? ingredients.push(element.id)
            : ingredients.push(element.name);
        }
      });
      flag === true
        ? ingredients.push(this.cheese.id)
        : ingredients.push("Queso");

      return ingredients;
    },
    arrayToString(ingredients) {
      let ingredientsString = "";
      ingredients.forEach((element) => {
        ingredientsString += element + ",";
      });
      return ingredientsString.substring(0, ingredientsString.length - 1);
    },
    link(size) {
      if (size === "Triple") {
        return "https://static.vix.com/es/sites/default/files/imj/elgranchef/5/5597134000_d3c91a42e6_z.jpg";
      } else if (size === "Doble") {
        return "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcR4XJG9yfRrg0LHR1TyMzUyfSNCmZBSSO_j6DBCCUHil1jaMFXRZ1L5QBhqqnmSp-qaeXI&usqp=CAU";
      } else if (size === "Individual") {
        return "https://previews.123rf.com/images/gospix/gospix1705/gospix170500160/77885465-s%C3%A1ndwich-individual.jpg";
      }
    },
    remove(id) {
      this.sandwichesAdded = this.sandwichesAdded.filter(
        (item) => item.id !== id
      );
    },
    confirm() {
      this.dialog = false;
      EventBus.$emit("order-confirm", this.sandwichesAdded, this.getTotal);
    },
  },
};
</script>
