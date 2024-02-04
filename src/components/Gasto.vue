<script setup>
import { formatearCantidad, formatearFecha } from '../helpers';
import IconoAhorro from '../assets/icono_ahorro.svg'
import IconoCasa from '../assets/icono_casa.svg'
import IconoComida from '../assets/icono_comida.svg'
import IconoGastos from '../assets/icono_gastos.svg'
import IconoOcio from '../assets/icono_ocio.svg'
import IconoSalud from '../assets/icono_salud.svg'
import IconoSuscripciones from '../assets/icono_suscripciones.svg'

const diccionarioIconos = {
    ahorro: IconoAhorro,
    comida: IconoComida,
    casa: IconoCasa,
    gastos: IconoGastos,
    ocio: IconoOcio,
    salud: IconoSalud,
    suscripciones: IconoSuscripciones
}

const emits = defineEmits(['seleccionar-gasto'])
const props = defineProps({
    gasto: {
        type: Object,
        required: true
    }
})
</script>

<template>
    <div class="gasto sombra" @click="emits('seleccionar-gasto', gasto.id)">
        <div class="contenido">
            <img :src="diccionarioIconos[gasto.categoria]" alt="Icono gasto" class="icono">
            <div class="detalles">
                <p class="categoria">{{ gasto.categoria }}</p>
                <p class="nombre">{{ gasto.nombre }}</p>
                <p class="fecha">
                    Fecha 
                    <span>{{ formatearFecha(gasto.fecha) }}</span>
                </p>
            </div>
        </div>
        <p class="cantidad">{{ formatearCantidad(gasto.cantidad) }}</p>
    </div>
</template>
    

<style scoped>
.gasto {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 2rem;
    cursor: pointer;
}

.contenido {
    display: flex;
    align-items: center;
    gap: 2rem;
}

.icono{
    width: 5rem;
}
.detalles p {
    margin: 0 0 1rem 0;
}

.categoria {
    color: var(--gris);
    font-size: 1.2rem;
    text-transform: uppercase;
    font-weight: 900;
}

.nombre {
    color: var(--gris-oscuro);
    font-size: 2.4rem;
    font-weight: 700;
}

.fecha {
    font-size: 1.6rem;
    font-weight: 900;
}

.fecha span{
    font-weight: 400;
    color: var(--gris-oscuro);
}

.cantidad{
    font-size: 2.5rem;
    font-weight: 700;
    margin: 0;
}
</style>