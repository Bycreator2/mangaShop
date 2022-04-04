<script setup>
import { RouterLink, RouterView } from 'vue-router'
import VueCookies from 'vue-cookies';
</script>

<template>
  <nav class="navbar sticky-top navbar-dark bg-dark">
      <div class="container-fluid">

        <RouterLink class="navbar-brand logowtext" to="/" style="display: flex;">
          <h5 style="font-weight: bold;">
            <img src="/src/assets/logoo.png" alt="" width="60" height="48" class="d-inline-block align-text-center logo">
            MangaShop
          </h5>
        </RouterLink>

        <div>
        <button class="btn btn-light" type="button" data-bs-toggle="offcanvas" data-bs-target="#Profilo" aria-controls="offcanvasRight" @click="updateCarello" >👤</button>
        &nbsp;
        <button class="btn btn-light" type="button" data-bs-toggle="offcanvas" data-bs-target="#Carrello" aria-controls="offcanvasRight" @click="updateCarello" >🛒 <span class="badge badge-secondary" style="color: black">{{carrello.length}}</span> </button>
        </div>


      </div>
  </nav>

  <div class="container">
    
    <!-- CARRELLO -->
    <div class="offcanvas offcanvas-end" tabindex="-1" id="Carrello" aria-labelledby="offcanvasRightLabel">
      <div class="offcanvas-header">
        <h5 id="offcanvasRightLabel">Carrello <span class="badge badge-dark">{{carrello.length}}</span> </h5>
        <button type="button" class="btn-close text-reset" data-bs-dismiss="offcanvas" aria-label="Close"></button>
      </div>

      <div class="offcanvas-body">

        <div v-for="item in carrello" :key="item.id" style="padding-bottom: 1rem;">

          <div style="background-color: #808080; border-radius: 10px; height: 90px;">
            <div style="width: 100%; padding: 10px; display: inline-flex; align-items: center;">
              <div style="padding-right: 10px;">
                <img
                  style="height: 70px!important; object-fit:cover; border-radius: 5px;"
                  :src="datiRicevuti[item.id].foto"
                />
              </div>
              <div style="display: inline-flex; text-align: left; position:relative; overflow: hidden; padding: 10px">
                <h5 class="cropText" style="width: fit-content; margin-right: 50px; ">{{ datiRicevuti[item.id].Nome }}
                  <br>
                  <h6 class="cropText" style="width: fit-content; margin-right: 50px;">€{{ datiRicevuti[item.id].prezzo }}</h6>
                </h5>
                
                <button @click="removeItem" :id="item.id" class="btn btn-primary" style="position: absolute; right: 0;">-</button>
              </div>
            </div>
          </div>

        </div>

        <form action="/create-checkout-session" method="POST">
          <button type="submit" id="checkout-button" class="btn btn-warning">Checkout €{{totPrezzo}}</button>
        </form>

      </div>

    </div>

    <!-- PROFILO -->
    <div class="offcanvas offcanvas-end" tabindex="-1" id="Profilo" aria-labelledby="offcanvasRightLabel">
      <div class="offcanvas-header">
        <h5 id="offcanvasRightLabel">Profilo <span class="badge badge-dark">{{carrello.length}}</span> </h5>
        <button type="button" class="btn-close text-reset" data-bs-dismiss="offcanvas" aria-label="Close"></button>
      </div>

      <div class="offcanvas-body">

        

        

      </div>

    </div>

    <RouterView />

  </div>

</template>


<script>
export default {
  data() {
    return {
      productIndex: null,
      datiRicevuti: null,
      carrello: 0,
      totPrezzo: 0
    }
  },
  async created() {
    // Simple GET request using fetch
    const response = await fetch("https://my.api.mockaroo.com/data.json?key=d60dbd50")
    const data = await response.json();
    this.datiRicevuti = data;
    this.productIndex = window.location.href.split("?")[1].split(" ")[0];
    if ($cookies.get("Carello") != null) {
      this.carrello = JSON.parse($cookies.get("Carello"))
    }
  },
  methods: {
    addCarello(event) {
      const thisProd = this.productIndex
      if ($cookies.get("Carello") == null) {
        $cookies.set("Carello", JSON.stringify([{ id: thisProd }]), "10y")
      } else {
        let cart = JSON.parse($cookies.get("Carello"));
        cart.push({ id: thisProd });
        $cookies.set("Carello", JSON.stringify(cart), "10y")
        console.log(JSON.parse($cookies.get("Carello")))
      }
    },
    updateCarello(event) {
      if($cookies.get("Carello") != []){
        this.carrello = JSON.parse($cookies.get("Carello"));
        let total = 0
        for (let id in this.carrello) {
          console.log("pod carrello: " + this.carrello[id].id + " prezzo: " +this.datiRicevuti[id].prezzo)
          total = total + this.datiRicevuti[id].prezzo;
          console.log(total)
          this.totPrezzo = total;
        }
      }else{
        this.totPrezzo = 0;
        console.log("vuoto")
      }
    },
    removeItem(event) {

      if (event) {
        console.log(this.carrello.find( ({id}) => id === event.target.id))
        this.carrello.splice(this.carrello.indexOf(this.carrello.find( ({id}) => id === event.target.id)), 1)
        $cookies.set("Carello", JSON.stringify(this.carrello), "10y")
      }

      if($cookies.get("Carello") != []){
        this.carrello = JSON.parse($cookies.get("Carello"));
          let total = 0
          for (let id in this.carrello) {
            console.log("pod carrello: " + this.carrello[id].id + " prezzo: " +this.datiRicevuti[id].prezzo)
            total = total + this.datiRicevuti[id].prezzo;
            console.log(total)
            this.totPrezzo = total;
          }
      }else{
        this.totPrezzo = 0;
        console.log("vuoto")
      }

    }
  }
}
</script>



<style>
@import url("https://fonts.googleapis.com/css2?family=Inter:wght@400;700;800&display=swap");

h1, h2, h3, h4, h5, h6, p{
  font-family: Inter;
}

body {
  align-content: center;
  justify-content: center;
  text-align: center;
  
  background: linear-gradient(rgba(33,37,41, 0.90), rgba(33,37,41, 0.90)), url("https://i.etsystatic.com/28121411/r/il/3d1a9e/3268896829/il_1140xN.3268896829_75g9.jpg");
  background-color: rgba(33,37,41);
}


.logo {
  image-rendering: -moz-crisp-edges;         /* Firefox */
  image-rendering:   -o-crisp-edges;         /* Opera */
  image-rendering: -webkit-optimize-contrast;/* Webkit (non-standard naming) */
  image-rendering: crisp-edges;
  -ms-interpolation-mode: nearest-neighbor;  /* IE (non-standard property) */
  object-fit: scale-down;

  -webkit-filter: drop-shadow(0px 0px 3px grey);
  filter: drop-shadow(0px 0px 3px grey);
  
}
.logowtext:hover{
  transition: all .1s ease-in-out;
  transform: scale(1.1);

  -webkit-filter: drop-shadow(0px 0px 4px grey);
  filter: drop-shadow(0px 0px 4px grey);
}
</style>