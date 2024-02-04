<script setup>
const props = defineProps(['filtro'])
const emits = defineEmits(['update:filtro'])
</script>

<template>
    <div class="filtros sombra contenedor">
        <form >
            <div class="campo">
                <label for="filtro">Filtrar gastos</label>
                <select id="filtro" :value="filtro" @change="emits('update:filtro', $event.target.value)">
                        <option value="">-- Selecciona --</option>
                        <option value="comida">Comida</option>
                        <option value="casa">Casa</option>
                        <option value="gastos">Gastos varios</option>
                        <option value="ocio">Ocio</option>
                        <option value="salud">Salud</option>
                        <option value="suscripciones">Suscripciones</option>
                    </select>
            </div>
        </form>
    </div>
</template>

<style scoped>
.filtros{
    margin-top: 10rem;
}
.filtros .campo{
    display: flex;
    align-items: center;
    gap: 2rem;
}
.filtros label{
    font-size: 2.5rem;
    font-weight: 900;
    color: var(--gris-oscuro);
}
.filtros select {
    flex: 1;
    padding: 1rem;
    border: none;
    border-radius: 1rem;
    background-color: var(--gris-claro);
    text-align: center;
}
</style>