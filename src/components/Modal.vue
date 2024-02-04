<script setup>
import { ref } from 'vue';
import Alerta from './Alerta.vue'
import iconoCerrarModal from '../assets/cerrar.svg'

const error = ref('')

const emits = defineEmits(['ocultar-modal', 'guardar-gasto', 'update:nombre', 'update:cantidad', 'update:categoria', 'eliminar-gasto'])
const props = defineProps({
    id: {
        type: [String, null],
        required: true
    },
    modal: {
        type: Object,
        required: true
    },
    nombre: {
        type: String,
        required: true
    },
    cantidad: {
        type: [Number],
        required: true
    },
    categoria: {
        type: String,
        required: true
    },
    disponible: {
        type: Number,
        required: true
    }
})

const agregarGasto = () => {
    const { cantidad, nombre, categoria, disponible, id } = props;
    if ([cantidad, nombre, categoria].some(s => String(s).trim() === "")) {
        error.value = 'Todos los campos son obligatorios'
        setTimeout(() => {
            error.value = ''
        }, 3000);
        return
    } 
    
    if (id) {
        if( cantidad > props.cantidad + disponible){
            error.value = 'La cantidad debe ser mayor a 0'
            setTimeout(() => {
                error.value = ''
            }, 3000);
            return
        }
    } else {
        if (cantidad <= 0) {
            error.value = 'La cantidad debe ser mayor a 0'
            setTimeout(() => {
                error.value = ''
            }, 3000);
            return
        }
    }

    if (cantidad > disponible) {
        error.value = 'Has excedido el presupuesto'
        setTimeout(() => {
            error.value = ''
        }, 3000);
        return
    }
    emits('guardar-gasto')
}
</script>

<template>
    <div class="modal">
        <div class="cerrar-modal">
            <img :src="iconoCerrarModal" @click="emits('ocultar-modal')" />
        </div>
        <div class="contenedor contenedor-formulario" :class="[modal.animar ? 'animar' : 'cerrar']">
            <form class="nuevo-gasto" @submit.prevent="agregarGasto">
                <legend>{{id? 'Editar gasto' : 'Añadir gasto'}}</legend>
                <Alerta v-if="error">
                    {{ error }}
                </Alerta>
                <div class="campo">
                    <label for="nombre">Nombre gasto: </label>
                    <input type="text" id="nombre" placeholder="Añade el nombre del gasto..." :value="props.nombre"
                        @input="emits('update:nombre', $event.target.value)">
                </div>
                <div class="campo">
                    <label for="cantidad">Cantidad: </label>
                    <input type="number" id="cantidad" placeholder="Añade la cantidad del gasto..." :value="cantidad"
                        @input="emits('update:cantidad', Number($event.target.value))">
                </div>
                <div class="campo">
                    <label for="categoria">Categoría: </label>
                    <select id="categoria" :value="categoria" @change="emits('update:categoria', $event.target.value)">
                        <option value="">-- Selecciona --</option>
                        <option value="comida">Comida</option>
                        <option value="casa">Casa</option>
                        <option value="gastos">Gastos varios</option>
                        <option value="ocio">Ocio</option>
                        <option value="salud">Salud</option>
                        <option value="suscripciones">Suscripciones</option>
                    </select>
                </div>

                <input type="submit" :value="id? 'Editar gasto' : 'Añadir gasto'">
                <button v-if="id" type="button" class="btn-eliminar" @click="emits('eliminar-gasto', id)">Eliminar gasto</button>
            </form>
        </div>
    </div>
</template>

<style scoped>
.modal {
    position: absolute;
    background-color: rgb(0 0 0 / 0.9);
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
}

.cerrar-modal {
    position: absolute;
    right: 3rem;
    top: 3rem;
}

.cerrar-modal img {
    width: 3rem;
    cursor: pointer;
}

.contenedor-formulario {
    transition-property: all;
    transition-duration: 300ms;
    transition-timing-function: ease-in;
    opacity: 0;
}

.contenedor-formulario.animar {
    opacity: 1;
}

.contenedor-formulario.cerrar {
    opacity: 0;
}

.nuevo-gasto {
    margin: 10rem auto 0 auto;
    display: grid;
    gap: 2rem;
}

.nuevo-gasto legend {
    text-align: center;
    color: var(--blanco);
    font-size: 3rem;
    font-weight: 700;
}

.campo {
    display: grid;
    gap: 2rem;
}

.nuevo-gasto input,
.nuevo-gasto select {
    background-color: var(--gris-claro);
    border-radius: 1rem;
    padding: 1rem;
    border: none;
    font-size: 2.2rem;
}

.nuevo-gasto label {
    color: var(--blanco);
    font-size: 3rem;
}

.nuevo-gasto input[type='submit'] {
    background-color: var(--azul);
    color: var(--blanco);
    font-weight: 700;
    cursor: pointer;
}
.btn-eliminar{
    padding: 1rem;
    width: 100%;
    background-color: rgb(126, 0, 0);
    font-weight: 700;
    font-size: 1.2rem;
    color: var(--blanco);
    margin-top: 10rem;
    cursor: pointer;
    border: none;
}
</style>