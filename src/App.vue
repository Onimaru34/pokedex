<template>

  <div :style="{ backgroundColor: getColor(pokType2[0]) }" class="container">

    <div id="secciones">

    
    <div class="q-pa-md">
      <div class="q-gutter-md" style="max-width: 300px">
        <q-input outlined v-model="pokemonId" label="pokemon" @keyup.enter="traer()"/>
        <button id="boton" @click="traer()" class="styled-button">Yo te elijo!!</button>
      </div>
    </div>

    <div id="subparte">
    <div id="nombreImg" v-if="pokeApi">
        <h1>Nombre: {{ nombre }}</h1>
        <img :src="imgPokemon" alt="" />
    </div>

     <div id="nombres" v-if="pokeApi">
        <h1>Altura: {{ altura }}</h1>
        <h1>Peso: {{ peso }}</h1>
        <div class="tipos-container">
          <div v-for="tipo in pokType2" :key="tipo" class="tipo-container">
            <span>Tipo:</span>
            <h1 id="tipos" class="tipo-text" :style="{ backgroundColor: getColor(tipo) }"> {{ tipo }}</h1>
          </div>
        </div>
      </div>
      </div>
    </div>
    

    

      <div id="cajas" class="q-pa-md flex flex-center stats-grid" v-if="pokeApi">
        <div v-for="stat in stats" :key="stat.stat.name" class="stat-container">
          <h2>{{ stat.stat.name }}: {{ stat.base_stat }}</h2>
          <q-circular-progress
            :value="stat.base_stat"
            size="85px"
            :thickness="0.6"
            color="red"
            center-color="white"
            class="q-ma-md"
          />
        </div>
      </div>

      
    

    <div id="id" v-if="pokeApi">#ID: {{ id }}</div>
  </div>
</template>

<script setup>
import axios from "axios";
import { ref } from "vue";

let pokemonId = ref("");
let imgPokemon = ref("");
let nombre = ref("");
let id = ref("");
let altura = ref("");
let peso = ref("");
let tipo = ref("");
let stats = ref([]);
let pokeApi = ref(false);
let pokType2 = ref([]);

const coloresTipos = {
  normal: "#A8A878",
  fire: "#F08030",
  water: "#6890F0",
  grass: "#78C850",
  electric: "#F8D030",
  ice: "#98D8D8",
  fighting: "#C03028",
  poison: "#A040A0",
  ground: "#E0C068",
  flying: "#A890F0",
  psychic: "#F85888",
  bug: "#A8B820",
  rock: "#B8A038",
  ghost: "#705898",
  dragon: "#A29BFE",
  dark: "#705848",
  steel: "#B8B8D0",
};

function getColor(type) {
  return coloresTipos[type] || "white"; // Si no se encuentra el color, devolver blanco
}

async function traer() {
  pokeApi.value = true;
  try {
    let res = await axios.get(
      `https://pokeapi.co/api/v2/pokemon/${pokemonId.value}`
    );
    imgPokemon.value = res.data.sprites.other["official-artwork"].front_default;
    nombre.value = res.data.name;
    id.value = res.data.id;
    altura.value = res.data.height;
    peso.value = res.data.weight;
    tipo.value = res.data.types.map(t => t.type.name).join(", ");
    stats.value = res.data.stats;
    pokType2.value = res.data.types.map((typeInfo) => typeInfo.type.name);
  } catch (error) {
    console.log(error);
  }
}
</script>

<style scoped>
.container {
  margin:0px;
  padding: 20px;
  border-radius: 10px;
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: center;
  height: 100vh; /* Establece la altura del contenedor al 100% de la ventana */ 
}

.q-pa-md {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-left:20px;
  align-items: flex-start;
}

.q-gutter-md {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-top: 10rem;
}

#boton {
  background-color: rgb(141, 26, 26);
  color: white;
  z-index: 20;
}

#secciones {
  display: flex;
  flex-direction: column;
  gap: 20px;
  margin-bottom: 260px;
}

#subparte{
  display: flex;
  flex-direction: row;
  gap:50px;
}

#id {
  position: absolute;
  top: 10px;
  right: 800px;
  color: red; 
  font-size: 10rem;
  z-index: 0;
}

#nombreImg h1 {
  font-size: 4rem;
}

#nombreImg img {
  height: auto;
  width: auto;
}

#nombres h1 {
  font-size: 3rem;
}

.tipos-container {
  display: flex;
  gap: 10px;
}

.tipo-container {
  padding: 5px 10px;
  border-radius: 5px;
  color: white;
  text-align: center;
}

.tipo-container span {
  font-size: 3rem;
}

#tipos {
  border-radius: solid 10px;
  border-color: white;
}

.tipo-text {
  margin: 0;
  padding: 5px 10px;
  border-radius: 5px;
  color: white;
  background-color: inherit; /* Usa el color del contenedor */
  border: 2px solid; /* Añadir borde */
  border-color: inherit; /* Usa el color del contenedor para el borde */
}

.stats-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 5px;
  width: 100%;
  justify-items: center;
}

.stat-container {
  display: flex;
  flex-direction: column;
  align-items: center;
}

#cajas {
  margin-bottom: 200px;
  width:40%;
  
  ;
}

.stat-container h2 {
  font-size: 26px; /* Cambia el tamaño según sea necesario */
  font-weight: 800;
  
}


.styled-button {
  background-color: #4CAF50; /* Green */
  border: none;
  color: white;
  padding: 10px 24px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
}

</style>