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
            :max="max">
            </v-date-picker>
        </v-row>
      </v-card>
      <v-card color="error" dark>
        <v-card-text class="display-1 text-center">
          {{dll}}
        </v-card-text>
      </v-card>
    </v-flex>
  </v-layout>
</template>

<script>
import axios from 'axios';
export default {
  name: "Home",
  components: {},
  data: () => ({
    date: new Date().toISOString().substr(0, 10),
    min: '1984',
    max: new Date().toISOString().substr(0, 10),
    dll: '',
  }),
  methods:{
    async getDolar(){
      //https://mindicador.cl/api/dolar
      let x = this.max.split("").reverse().join("");
      x = x.substr(0,6) + x.substr(6, 10).split("").reverse().join("");
      let datos =  await axios.get(`https://mindicador.cl/api/dolar/${x}`);
      this.dll = await datos.data.serie[0].valor;
    },
  },
  created() {
    this.getDolar();
  },
};
</script>
