<template>
  <div id="app" class="fondo my-4">
    <div class="container">
      <header class="text-center">
        <img src="/img/Pokemon-23.svg" width="60%" alt="">
        <div class="bg-container bg-white">
          <h2 class="py-3">¿Quien es ese Pokemon?</h2>
          <h5 class="">Pokemones descubiertos: <span>{{ contador }}</span></h5>
        </div>
      </header>
      <main class="container py-3">
        <!-- envolvimos en un div el div del v-for para ordenar las cards -->
        <div class="row g-3">
          <!-- incluimos v-for para recorrer el array de pokemones -->
          <div class="col-12 col-md-6 col-lg-3" v-for="(pokemon, index) in pokemones" :key="index">
            <!-- ahora usamos data binding del prop, a la izq el nombre del props y a la derecha el valor que se entrega -->
            <CardPoke :pokemon="pokemon" @nombreCorrecto="incrementar" />
          </div>
        </div>
      </main>
    </div>
  </div>
</template>

<script>
import CardPoke from './components/CardPoke.vue';
// instalamos en terminal axios e importamos para llamar las apis
import axios from 'axios';

export default {
  name: 'App',
  components: {
    CardPoke,
  },
  // con data incorporamos variables (estados) a usar
  data() {
    return {
      pokemones: [],
      contador: 0,
    }
  },
  // method lugar donde almacenar las funciones a ejecutar
  methods: {
    // para el manejo de funciones de llamado de api se usan asyncronia (async), usamos axios.
    // https://pokeapi.co/api/v2/pokemon -> original
    async getPoke() {
      try {
        let response = await axios.get("https://pokeapi.co/api/v2/pokemon?limit=20&offset=500");
        // console.log(response)
        // con este codigo linkeamos el llamado de la APi a la variable "array" Pokemones del data
        this.pokemones = response.data.results;

      } catch (error) {
        console.log(error)
      }
    },
    incrementar() {
      this.contador++;
    }
  },
  // hacemos el "mounted" para poder llamar la funcion al momento se ser montado el servidor
  mounted() {
    this.getPoke();
  }
}
</script>

<style>
* {
  font-family: "Press Start 2P", system-ui;
  font-weight: 400;
  font-style: normal;
}

.bg-container{
  border: 1px solid black
}

.fondo {
  background-image: url(../src/assets/img/Rpg.jpg);
  background-position: top;
  background-attachment: scroll;
  background-size: cover;
}
</style>
