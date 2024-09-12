<script>
//1- Importazione del componenente necessario che voglio aggiungere qua
import axios from 'axios';
import AppHeader from './components/AppHeader.vue';

export default {
  data() {
    return {
        allCards:[],
        
    }
  },
  created(){
    axios
    .get('https://db.ygoprodeck.com/api/v7/cardinfo.php?num=20&offset=0')
    .then((res)=> {
      console.log(res);
      console.log(res.data);
      console.log(res.data.data);
      
      this.allCards = res.data.data;
      console.log(this.allCards);
    });
  },
  //2- Dichiarazione del componente 
  components: {
    AppHeader
  },
  methods: {
  }
}
</script>

<template>
  <div class="bg-warning">
    <!--3- Utilizzo del componenete -->
    <AppHeader />
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
