<script setup>
import { onMounted, reactive, ref } from "vue";
import { db } from "./data/guitarras";
import Guitarra from "./components/Guitarra.vue";
import Header from "./components/Header.vue";
import Footer from "./components/Footer.vue";
//Usar reactive para objetos relacionados
//Usar REF para arrays y llamadas a API
const guitarras = ref([]);
const carrito = ref([]);

onMounted(() => {
    guitarras.value = db;
});

//Usamos findIndex para saber si el producto ya existe en el carrito,devuelve -1 si no existe
const agregarCarrito = (guitarra) => {
    const existeCarrito = carrito.value.findIndex(item=> item.id === guitarra.id);
    if(existeCarrito >=0){
        carrito.value[existeCarrito].cantidad++;
    } else{
        guitarra.cantidad = 1;
        carrito.value.push(guitarra);
    }
    console.log(existeCarrito);

};
</script>

<template>
    <Header
    :carrito = "carrito"
    />
    <main class="container-xl mt-5">
        <h2 class="text-center">Nuestra Colección</h2>
        <div class="row mt-5">
            <Guitarra
                v-for="guitarra in guitarras"
                :guitarra="guitarra"
                @agregarCarrito="agregarCarrito"
            />
        </div>
    </main>
    <Footer />
</template>
