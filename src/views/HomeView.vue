<script setup>
import { RouterLink, RouterView } from 'vue-router'
import ProductCard from '../components/ProductCard.vue'
</script>

<template>
    <div class="container">
      <br>
      <h1>Benvenuto in MangaShop!!</h1>
    
      <div v-if="datiRicevuti != null">

        <!--NUOVIARRIVI-->
        <div class="product">
          <h4 style="text-align:start;">nuovi arrivi📣</h4>
          
            <div class="row row-cols-2 row-cols-md-2 row-cols-xl-4 g-2">
              <div v-for="item in datiRicevuti.slice(0, 4)" :key="item.id" class="col" style="padding-bottom: 1rem;">

                <!--<div class="card" style="height: 25rem;">
                  <img :src="item.foto" class="card-img-top" alt="..." style="height: 55%!important; object-fit:cover;">
                  <div class="card-body" style="justify-content: center; align-content: center;">
                    <h5 class="card-title cropText">{{item.Nome}}</h5>
                    <p class="card-text">€{{item.prezzo}}</p>
                    <RouterLink :to="'/products?' + item.id" class="btn btn-secondary card-text">Altre info</RouterLink>
                  </div>
                </div>-->
                <product-card :foto="item.foto" :Nome="item.Nome" :prezzo="item.prezzo" :id="item.id"></product-card>

              </div>
            </div>
            
        </div>
          
        <!--OCCASIONI-->
        <div class="product">
          <h4 style="text-align:start;">occasioni💰</h4>
        
            <div class="row row-cols-2 row-cols-md-4 g-2">
              <div v-for="item in datiRicevuti.slice(0, 4)" :key="item.id" class="col" style="padding-bottom: 1rem;">

                <product-card :foto="item.foto" :Nome="item.Nome" :prezzo="item.prezzo" :id="item.id"></product-card>

              </div>
            </div>
            
        </div>

      </div>
      <div v-else style="padding-top: 50%;">
        <h1 style="font-size:medium;">🚧Stiamo caricando la home🚧</h1>
        <div class="progress" style="width: 100%!important;">
          <div class="progress-bar progress-bar-striped progress-bar-animated" role="progressbar" style="width: 100%"></div>
        </div>
      </div>

    </div>


</template>


<script>
  export default{
    name: "get-request-async-await",
    data() {
      return {
        datiRicevuti: null
      };
    },
    async created() {
      // Simple GET request using fetch
      const response = await fetch("https://my.api.mockaroo.com/data.json?key=d60dbd50")
        const data = await response.json();
        this.datiRicevuti = data;
    },
    components: {
      ProductCard
    }
  }
</script>

<style>

  h1,h4{ 
    color: white;
    font-family: 'Inter', sans-serif;
    font-weight: 1000;
  }
  

  .cropText {
    max-width: 30em;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
}
</style>