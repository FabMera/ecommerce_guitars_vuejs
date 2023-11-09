<script setup>
import { onMounted, ref, watch } from "vue";
import { db } from "./data/guitarras";
import Guitarra from "./components/Guitarra.vue";
import Header from "./components/Header.vue";
import Footer from "./components/Footer.vue";

//Usar reactive para objetos relacionados
//Usar REF para arrays y llamadas a API
const guitarras = ref([]);
const carrito = ref([]);
const guitarra = ref({});

watch(
    carrito,
    () => {
        guardarLocalStorage();
    },
    {
        deep: true,
    }
);

onMounted(() => {
    guitarras.value = db;
    //mostrar la guitarra que aparece en la pagina principal "Modelo VAI"
    guitarra.value = db[3];
    const carritoStorage = JSON.parse(localStorage.getItem("carrito"));
    if (carritoStorage) {
        carrito.value = carritoStorage;
    }
});
const guardarLocalStorage = () => {
    localStorage.setItem("carrito", JSON.stringify(carrito.value));
};

//Usamos findIndex para saber si el producto ya existe en el carrito,devuelve -1 si no existe
const agregarCarrito = (guitarra) => {
    const existeCarrito = carrito.value.findIndex(
        (item) => item.id === guitarra.id
    );
    if (existeCarrito >= 0) {
        carrito.value[existeCarrito].cantidad++;
    } else {
        guitarra.cantidad = 1;
        carrito.value.push(guitarra);
    }
    guardarLocalStorage();
};
const decrementarCantidad = (id) => {
    const index = carrito.value.findIndex((item) => item.id === id);
    if (carrito.value[index].cantidad <= 1) return;
    carrito.value[index].cantidad--;
};
const incrementarCantidad = (id) => {
    const index = carrito.value.findIndex((item) => item.id === id);
    if (carrito.value[index].cantidad >= 5)
        return alert("No puedes agregar mas de 5 productos");
    carrito.value[index].cantidad++;
};

const resultadoCarrito = () => {
    const resultado = carrito.value.reduce(
        (total, producto) => total + producto.precio * producto.cantidad,
        0
    );
    return resultado;
};

const eliminarProducto = (id) => {
    const index = carrito.value.findIndex((item) => item.id === id);
    carrito.value.splice(index, 1);
};

const vaciarCarrito = () => {
    carrito.value = [];
};
</script>

<template>
    <Header
        :carrito="carrito"
        :resultadoCarrito="resultadoCarrito"
        :guitarra="guitarra"
        @agregar-carrito="agregarCarrito"
        @incrementar-cantidad="incrementarCantidad"
        @decrementar-cantidad="decrementarCantidad"
        @eliminar-producto="eliminarProducto"
        @vaciar-carrito="vaciarCarrito"
    />
    <main class="container-xl mt-5">
        <h2 class="text-center">Nuestra Colección</h2>
        <div class="row mt-5">
            <Guitarra
                v-for="guitarra in guitarras"
                :guitarra="guitarra"
                @agregar-carrito="agregarCarrito"
            />
        </div>
    </main>
    <Footer />
</template>
