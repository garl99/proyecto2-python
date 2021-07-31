<template>
  <div class="about">
    <add-sandwich></add-sandwich>
    <v-container>
      <v-row class="mt-5">
        <v-col v-if="success" cols="4">
          <v-alert dense text type="success"> Pedido creado con exito </v-alert>
        </v-col>
        <v-col v-if="error" cols="4">
          <v-alert dense text type="error"> Ha ocurrido un error </v-alert>
        </v-col>
        <v-col cols="12">
          <h1 class="text-center">Resumen del pedido</h1></v-col
        >
      </v-row>
      <v-row class="mt-5">
        <v-col class="mb-5" cols="12">
          <h3>Datos del cliente</h3>
          <v-form>
            <v-container>
              <v-row>
                <v-col cols="12" sm="6" md="3">
                  <v-text-field
                    v-model="customer.dni"
                    label="DNI"
                    type="number"
                  ></v-text-field>
                </v-col>
                <v-col cols="12" sm="6" md="3">
                  <v-text-field
                    v-model="customer.name"
                    label="Nombre"
                  ></v-text-field>
                </v-col>
                <v-col cols="12" sm="6" md="3">
                  <v-text-field
                    v-model="customer.lastname"
                    label="Apellido"
                  ></v-text-field>
                </v-col>
              </v-row>
            </v-container>
          </v-form>
        </v-col>
      </v-row>
      <v-divider inset></v-divider>
      <v-row class="mt-5">
        <v-col class="mb-5 mt-5" cols="12">
          <div style="display: flex">
            <h3>Sanwiches agregados</h3>
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
          </div>
          <div
            v-if="order.length !== 0"
            class="mb-5 mt-5"
            style="display: flex; flex-wrap: wrap"
          >
            <v-card
              v-for="item in order"
              :key="item.id"
              class="mx-5 mb-5"
              max-width="344"
              elevation="2"
              style="border-radius: 10px; position: relative"
              outlined
            >
              <v-list-item three-line>
                <v-list-item-content>
                  <div class="text-overline mb-4">#{{ item.id }}</div>
                  <v-list-item-title class="text-h5 mb-1">
                    Sandwich {{ item.sizeLabel }}
                  </v-list-item-title>
                  <v-list-item-subtitle
                    ><strong>Ingredientes: </strong
                    >{{
                      arrayToString(item.ingredientsLabels)
                    }}</v-list-item-subtitle
                  >
                </v-list-item-content>

                <v-list-item-avatar size="80">
                  <v-img :src="link(item.sizeLabel)"></v-img>
                </v-list-item-avatar>
              </v-list-item>

              <v-card-actions
                style="display: flex; justify-content: space-between"
              >
                <v-btn icon @click="remove(item.id)">
                  <v-icon color="red lighten-1">mdi-delete</v-icon>
                </v-btn>
                <p style="margin: 0">${{ item.subtotal }}</p>
              </v-card-actions>
            </v-card>
          </div>
          <div
            v-else
            class="mt-5"
            style="
              width: 100%;
              display: flex;
              justify-content: center;
              align-items: center;
              flex-direction: column;
            "
          >
            <img src="@/assets/icons8-sad.gif" alt="sad" width="150" />
            <p style="font-size: 16px; font-weight: bold; text-align: center">
              No ha agregado ningún sandwich
            </p>
          </div>
        </v-col>
      </v-row>
      <v-row>
        <v-col
          class="mb-5 mt-5"
          cols="12"
          style="display: flex; justify-content: flex-end"
        >
          <div style="display: flex; flex-direction: column">
            <p style="margin: 0; font-size: 16px; color: #000">
              <strong>Total</strong>
            </p>
            <span>$ {{ getTotal }}</span>
          </div>
        </v-col>
      </v-row>
      <v-row v-if="order.length !== 0">
        <v-col
          class="mb-5 mt-5"
          cols="12"
          style="display: flex; justify-content: center"
        >
          <v-btn
            color="green"
            elevation="2"
            :loading="loading"
            dark
            rounded
            small
            @click="pay()"
          >
            Pagar
          </v-btn>
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
  created() {
    EventBus.$on("order-confirm", (data, total) => {
      this.setOrders(data);
      this.total = total;
      console.log(this.order);
    });
  },
  data() {
    return {
      order: [],
      total: 0,
      customer: {
        dni: null,
        name: null,
        lastname: null,
      },
      loading: false,
      success: false,
      error: false,
    };
  },
  computed: {
    getTotal() {
      let total = 0;
      this.order.forEach((element) => {
        total += element.subtotal;
      });
      return total;
    },
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
      this.currentTotal();
      EventBus.$emit("open-add-sandwich", this.total);
    },
    arrayToString(ingredients) {
      let ingredientsString = "";
      ingredients.forEach((element) => {
        ingredientsString += element + ",";
      });
      return ingredientsString.substring(0, ingredientsString.length - 1);
    },
    setOrders(data) {
      data.forEach((element) => {
        this.order.push(element);
      });
    },
    remove(id) {
      this.order = this.order.filter((item) => item.id !== id);
    },
    currentTotal() {
      let total = 0;
      this.order.forEach((element) => {
        total += element.subtotal;
      });
      this.total = total;
    },
    pay() {
      this.loading = true;
      let date = new Date();
      let dateString =
        date.getFullYear() +
        "-" +
        (date.getMonth() + 1 < 10
          ? "0" + (date.getMonth() + 1)
          : date.getMonth() + 1) +
        "-" +
        (date.getDate() < 10 ? "0" + date.getDate() : date.getDate());
      let data = {
        customer: this.customer,
        orderDetail: this.order,
        total: this.total,
        date: dateString,
      };
      var url = "http://186.90.151.12:5000/pedidos/";
      this.$axios
        .post(url, data)
        .then((response) => {
          this.loading = false;
          this.notify(true);
          this.resetValues();
          console.log(response);
        })
        .catch((error) => {
          this.loading = false;
          this.notify(false);
          console.log(error);
        });
    },
    notify(type) {
      if (type) {
        this.success = true;
        setTimeout(() => {
          this.success = false;
        }, 5000);
      } else {
        this.error = true;
        setTimeout(() => {
          this.error = false;
        }, 5000);
      }
    },
    resetValues() {
      this.order = [];
      this.total = 0;
      this.customer = {
        dni: null,
        name: null,
        lastname: null,
      };
    },
  },
};
</script>
