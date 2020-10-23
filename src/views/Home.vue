<template>
  <v-layout>
    <v-flex xs12>
      <v-card>
        <v-row justify="center">
          <v-date-picker
            v-model="date"
            full-width
            locale="es-mx"
            :min="min"
            :max="max"
            @change="getDolar(date)"
          >
          </v-date-picker>
        </v-row>
      </v-card>
      <v-card color="error" dark>
        <v-card-text class="display-1 text-center">
          {{ dll }}
        </v-card-text>
      </v-card>
    </v-flex>
  </v-layout>
</template>

<script>
import axios from "axios";
import { mapMutations } from "vuex";
export default {
  name: "Home",
  components: {},
  data: () => ({
    date: new Date().toISOString().substr(0, 10),
    min: "1984",
    max: new Date().toISOString().substr(0, 10),
    dll: "",
  }),
  methods: {
    ...mapMutations(['showLoading', 'hideLoading']),
    async getDolar(date) {
      let x = date.split("-").reverse().join("-");
      let datos = await axios.get(`https://mindicador.cl/api/dolar/${x}`);
        this.showLoading({titulo: 'Accediendo a la informacion', color: 'secondary'});
      try {
        if(datos.data.serie.length > 0){
          this.dll = await datos.data.serie[0].valor;
        }else if(x.split('-')[0] == 18){
          this.dll = "Dia de los duvalincitos";
        }else{
          this.dll = "Sin datos";
        }
      } catch (error) {
        console.log(error);
      } finally {
        setTimeout(() => {this.hideLoading()}, 500);
      }
    },
  },
  created() {
    this.getDolar(this.max);
  },
};
</script>
