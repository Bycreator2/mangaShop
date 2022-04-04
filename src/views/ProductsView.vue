<script setup>
import VueCookies from 'vue-cookies';
</script>

<template>
    <div v-if="datiRicevuti != null" class="container" style="margin-top: 10vh;">
        <div class="row row-cols-1 row-cols-xl-2 g-2">

            <div class="col">

                <img :src="datiRicevuti[productIndex].foto" style="width: 40vh;"/>
            
            </div>
            <div class="col" style="text-align: left;">

                <h1>{{datiRicevuti[productIndex].Nome}}</h1>

                <br/>

                <h5 style="color:white;">Valutazione: {{datiRicevuti[productIndex].valutazione}}/5</h5>

                <br/>

                <h4 style="color:white; font-weight: bold">€{{datiRicevuti[productIndex].prezzo}}</h4>

                <br/>
                
                <p style="font-weight:500; color:white;">{{datiRicevuti[productIndex].descrizione}}</p>
                <button type="button" class="btn btn-warning" @click="addCarello" id="liveToastBtn">Aggiungi al carello</button>
            
            </div>
        </div>

    </div>
    <div v-else style="padding-top: 50%;">
        <h1 style="font-size:medium;">🚧Stiamo caricando il prodotto🚧</h1>
        <div class="progress" style="width: 100%!important;">
            <div class="progress-bar progress-bar-striped progress-bar-animated" role="progressbar" style="width: 100%"></div>
        </div>
    </div>



    <div class="position-fixed bottom-0 end-0 p-3" style="z-index: 11">
        <div id="liveToast" class="toast fade hide" role="alert" aria-live="assertive" aria-atomic="true">
            <div class="toast-body">Prodotto aggiunto al carrello</div>
        </div>
    </div>



</template>

<script>
export default {
    data() {
        return {

            productIndex: null,
            datiRicevuti: null,
            carrello: JSON.parse($cookies.get("Carello")),
        }
    },
    async created() {
        const response = await fetch("https://my.api.mockaroo.com/data.json?key=d60dbd50")
        const data = await response.json();
        this.datiRicevuti = data;
        this.productIndex = window.location.href.split("?")[1].split(" ")[0];
        
    },
    methods: {
        addCarello(event) {
            const thisProd = this.productIndex

            if($cookies.get("Carello") == null){
                $cookies.set("Carello", JSON.stringify([{id: thisProd}]), "10y")
            }else{
                let cart = JSON.parse($cookies.get("Carello"));
                cart.push({id: thisProd});
                $cookies.set("Carello", JSON.stringify(cart), "10y")
                console.log(JSON.parse($cookies.get("Carello")))
            }
            const toastTrigger = document.getElementById('liveToastBtn')
            const toastLiveExample = document.getElementById('liveToast')
            toastTrigger.addEventListener('click', function () {
                var toast = new bootstrap.Toast(toastLiveExample)
                toast.show()
            })
        }
    }
}
</script>

<style>

</style>