<script setup>
//todo el codigo JS -Logica
import {ref,reactive,onMounted,watch} from 'vue';
import {db} from './data/guitarras'
import Guitarra from './components/guitarra.vue'
import Header from './components/Header.vue'
import Footer from './components/Footer.vue'

const guitarras=ref();
const carrito=ref([]);
const guitarra=ref({});

watch(carrito,()=>
{
    guardarLocalStorage();
    console.log("algo cambio");
},
{
    deep:true
})  

onMounted(()=>
{
    guitarras.value=db;
    guitarra.value=db[8];

    const carritoStorage=localStorage.getItem('carrito');
    if(carritoStorage)
    {
        carrito.value=JSON.parse(carritoStorage);
    }
})

const guardarLocalStorage=()=>
{
    localStorage.setItem('carrito',JSON.stringify(carrito.value))
}

const agregarCarrito=(guitarra)=>
    {
        const existeCarrito=carrito.value.findIndex(producto=>producto.id=== guitarra.id);
        if(existeCarrito>=0)
        {
        carrito.value[existeCarrito].Cantidad++;
        }
        else
        {
        guitarra.Cantidad=1;
        carrito.value.push(guitarra);
        }
    }  

    const decrementarCantidad=(id)=>
    {
        const index=carrito.value.findIndex(producto=>producto.id===id);
        if(carrito.value[index].Cantidad<=1) return
        carrito.value[index].Cantidad--;
      
    }  
    const incrementarCantidad=(id)=>
    {
        const index=carrito.value.findIndex(producto=>producto.id===id);
        if(carrito.value[index].Cantidad>=5) return
        carrito.value[index].Cantidad++;
    }      
    
    const eliminarProducto=(id)=>
    {
        //filter regresar
        carrito.value=carrito.value.filter(producto=>producto.id !== id)
    }

    const variarCarrito=()=>
    {
        carrito.value=[];
    }

</script>
<template>

<Header
:carrito="carrito"
:guitarra="guitarra"
@decrementar-cantidad="decrementarCantidad"
@incrementar-cantidad="incrementarCantidad"
@agregar-carrito="agregarCarrito"
@eliminar-producto="eliminarProducto"
@vaciar-carrito="variarCarrito"
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
<Footer/>
</template>

<style scoped>
</style>
