<script setup>
import { RouterLink, RouterView } from 'vue-router'
</script>

<template>
    <div class="card" style="height: 25rem; background-color: rgba(33,37,41); overflow: hidden; border-radius: 10px!important">
        <img
            :src="foto"
            class="card-img-top"
            alt="..."
            style="height: 100%!important; object-fit:cover; position: absolute; z-index: 0; opacity: 0.5;"
        />
        <div class="card-body" style="justify-content: center; align-content: center; background-color: transparent!important; z-index: 1;">
            <div style="position: absolute; bottom: 0; display: inline-grid; padding: 10px; left: 0; right: 0; color: white;">
                <h5 class="card-title cropText">{{ Nome }}</h5>
                <p class="card-text" style="font-weight: bold;">€{{ prezzo }}</p>
                <RouterLink :to="'/products?' + id" class="btn btn-dark card-text">Altre info</RouterLink>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    props: {
        foto: { required: true, type: String },
        Nome: { required: true, type: String },
        prezzo: { required: true, type: Number },
        id: { required: true, type: Number },
    }
}
</script>

<style>
</style>