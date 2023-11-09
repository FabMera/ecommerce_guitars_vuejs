<script setup>
const props = defineProps({
    carrito: {
        type: Array,
        required: true,
    },
    resultadoCarrito: {
        type: Function,
        required: true,
    },
    vaciarCarrito: {
        type: Function,
        required: true,
    },
});



defineEmits(['incrementar-cantidad', 'decrementar-cantidad', 'eliminar-producto', 'vaciar-carrito']);
</script>

<template>
    <div class="carrito">
        <img class="img-fluid" src="/img/carrito.png" alt="imagen carrito" />

        <div id="carrito" class="bg-white p-3">
            <p v-if="carrito.length === 0" class="text-center m-0">
                El carrito esta vacio
            </p>
            <div v-else>
                <table class="w-100 table">
                    <thead>
                        <tr>
                            <th>Imagen</th>
                            <th>Nombre</th>
                            <th>Precio</th>
                            <th>Cantidad</th>
                            <th></th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="producto in carrito">
                            <td>
                                <img
                                    class="img-fluid"
                                    :src="'/img/' + producto.imagen + '.jpg'"
                                    alt="imagen guitarra"
                                />
                            </td>
                            <td>{{ producto.nombre }}</td>
                            <td class="fw-bold">
                                {{ producto.precio }}
                            </td>
                            <td class="flex align-items-start gap-4">
                                <button
                                    @click="$emit('decrementar-cantidad',producto.id)"
                                    type="button"
                                    class="btn btn-dark"
                                >
                                    -
                                </button>
                                {{ producto.cantidad }}
                                <button
                                    @click="$emit('incrementar-cantidad',producto.id)"
                                    type="button"
                                    class="btn btn-dark"
                                >
                                    +
                                </button>
                            </td>
                            <td>
                                <butto @click="$emit('eliminar-producto',producto.id)" class="btn btn-danger" type="button">
                                    X
                                </butto>
                            </td>
                        </tr>
                    </tbody>
                </table>

                <p class="text-end">
                    Total pagar:
                    <span class="fw-bold">{{resultadoCarrito()}}</span>
                </p>
                <button
                    @click="$emit('vaciar-carrito')"
                    class="btn btn-dark w-100 mt-3 p-2"
                >
                    Vaciar Carrito
                </button>
            </div>
        </div>
    </div>
</template>
