<template>
  <div class="container text-center">
    <div class="card" style="width: 18rem;">
      <!-- listo todo lo anterior del <script> comenzamos a brindear en la card la info requerida -->
      <img :src="imagenPokemon" class="card-img-top" :class="esconder ? 'borroso' : ''" alt="...">
      <div class="card-body">
        <h5 class="card-title"></h5>
        <p class="card-text"></p>
        <!-- agregamos v-show o v-if al formulario de nombre y boton para que aparezca o desaparezca cuando es tru o false segun la variable asiganda -->
        <p v-show="!esconder">{{ pokemon.name }}</p>
        <form action="" v-show="esconder">
          <input class="form-control" type="text" v-model="descubrirPokemon" @keyup.enter="descubrir">
          <div class="d-grid my-2">
            <button @click.prevent="descubrir" class="btn btn-primary">Descubrir</button>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';


export default {
  name: 'CardPoke',
  props: {
    // usamos el prop de para comunicacion de padre a hijo y traemos el array pokemones[] del data App.vue (padre)
    pokemon: Object
  },
  data() {
    return {
      // creamos la variables que tomara los datos de la API
      infoPokemon: {},
      esconder: true,
      descubrirPokemon: "",
    }
  },
  // agregaremos el metodo computed para achicar el url para las imagenes
  computed: {
    imagenPokemon() {
      return this.infoPokemon.sprites?.other['official-artwork'].front_default;
    }
  },
  methods: {
    // esta funcion es solo para consumir la api especificamente la URL
    async getInfoPokemon() {
      try {
        // (1)aca el axios.get llamamos la props que ya traia la info y sacamos lo que necesitamos en este caso la URL
        // (2) usamos los {} para destructurar la variable data que ya tenia una data 
        let { data } = await axios.get(this.pokemon.url)
        // console.log(data) ->> console.log para revisar que llega la info, luego no se ocupa.
        // y aca linkeamos la info de la url y se la pasamos a la variable "infoPokemon" en data del componente
        this.infoPokemon = data;

      } catch (error) {
        console.log(error)
      }
    },
    // ahora desarrollamos la funcion que nos permita "descurbrir" el pokemon, al acertar el nombre usamos console.log en if y else para corroborar, luego no se ocupa por lo que se comenta
    descubrir() {
      if (this.descubrirPokemon.toLowerCase() === this.pokemon.name.toLowerCase()) {
        // console.log("correcto")
        this.esconder = false
        this.$emit("nombreCorrecto")
      } else {
        const name = this.pokemon.name;
        console.log("incorrecto");
        alert(`el nombre del pokemon comienza con: ${name.substring(0, 3)}`);
      }
    }
  },
  // (1)usamos ciclo de vida "created" para al momento de crear el componente se ejecuta lo que esta dentro,  "console.log" para revisar si la props esta correcto, esto solo para revision.
  // (2) cambiamos el console.log y activamos la funcion del metodo
  created() {
    this.getInfoPokemon()
  },
}
</script>


<style scoped>
.card {
  background-color: white;
}

.borroso {
  filter: blur(5px) grayscale(100%)
}

img {
  user-drag: none;
  -webkit-user-drag: none;
  user-select: none;
  -moz-user-select: none;
  -webkit-user-select: none;
  -ms-user-select: none;
}
</style>
