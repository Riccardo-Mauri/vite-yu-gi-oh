<script>
import axios from 'axios';
import AppHeader from './components/AppHeader.vue';

export default {
  data() {
    return {
      allCards: [],      // Array per le carte
      archetypes: [],    // Array per gli archetipi
      selectedArchetype: ''  // Variabile per l'archetipo selezionato
    };
  },
  created() {
    // Chiamata API per ottenere le carte (iniziale)
    this.fetchCards();

    // Chiamata API per ottenere gli archetipi delle carte
    axios.get('https://db.ygoprodeck.com/api/v7/archetypes.php')
      .then((res) => {
        this.archetypes = res.data;
      });
  },
  components: {
    AppHeader
  },
  methods: {
  // Metodo per recuperare le carte dalla API (con o senza archetipo)
  fetchCards(archetype = '') {
    let apiURL = 'https://db.ygoprodeck.com/api/v7/cardinfo.php?num=20&offset=0';
    
    // Se un archetipo è selezionato, aggiungere il parametro 'archetype'
    if (archetype) {
      apiURL += '&archetype=' + encodeURIComponent(archetype);
    }
    
    axios.get(apiURL)
      .then((res) => {
        this.allCards = res.data.data;
      });
  },
    // Metodo chiamato quando si cambia l'archetipo
    onArchetypeChange() {
      this.fetchCards(this.selectedArchetype);
    }
  }
};
</script>

<template>
  <div class="bg-warning">
    <!-- Header -->
    <AppHeader />
    
    <!-- Select per filtrare gli archetipi -->
    <div class="container bg-white py-3">
      <select v-model="selectedArchetype" @change="onArchetypeChange" class="form-select">
        <option value="">All Archetypes</option>
        <option v-for="(archetype, index) in archetypes" :key="index" :value="archetype.archetype_name">
          {{ archetype.archetype_name }}
        </option>
      </select>
    </div>

    <!-- Carte -->
    <div class="container bg-white">
      <div class="row justify-content-center">
        <div v-for="(cards, i) in allCards" :key="i" class="col-2 py-2 px-3 my-2 mx-2 text-center bg-warning">
          <div>
            <img :src="cards.card_images[0].image_url" alt="cards.name" class="img-thumbnail">
          </div>
          <div><h3 class="text-white fs-3 text">{{cards.name}}</h3></div>
          <div><h4 class="text-black-50 fs-5 text">{{cards.archetype}}</h4></div>
        </div>
      </div>
    </div>
  </div>
  
  <footer class="bg-warning">
    <!-- Calcolo delle carte caricate in pagina, in questo caso con valore massino di 20 e minimo di 0-->
    <div class="container">
      <div class="row">
        <div class="col text-center py-4">
          <p class="fs-3 fw-bold">Found {{ allCards.length }} cards!</p>
        </div>
      </div>
    </div>
  </footer>
</template>

<style lang="scss">
  @import 'bootstrap/scss/bootstrap';
</style>
