<script setup>
import { ref } from 'vue';
import Presupuesto from './components/Presupuesto.vue';
import FormularioGasto from './components/FormularioGasto.vue';
import ListadoGastos from './components/ListadoGastos.vue';

const presupuesto = ref(0);
const gastos = ref([]);

const definirPresupuesto = (cantidad) => {
  if(cantidad <= 0) {
    alert('El presupuesto debe ser mayor a 0');
    return false;
  }
  presupuesto.value = cantidad;
  return true;
};

const agregarGasto = (gasto) => {
  gastos.value.push(gasto);
};

const eliminarGasto = (id) => {
  gastos.value = gastos.value.filter(gasto => gasto.id !== id);
};

provide('presupuesto', presupuesto);
provide('gastos', gastos);
provide('disponible', disponible);
provide('gastado', gastado);
provide('agregarGasto', agregarGasto);
provide('eliminarGasto', eliminarGasto);
</script>

<template>
  <header>
    <h1>Planificador de Gastos</h1>
    <div class="contenedor sombra">
      <Presupuesto @definir-presupuesto="definirPresupuesto"/>
    </div>
  </header>

  <main v-if="presupuesto > 0" class="contenedor">
    <FormularioGasto />
    <ListadoGastos />
  </main>
</template>

<style>
:root {
  --azul: #3b82f6;
  --blanco: #FFF;
  --gris-claro: #F5F5F5;
  --gris: #94a3b8;
  --gris-oscuro: #64748b;
  --negro: #000;
}

html {
  font-size: 62.5%;
  box-sizing: border-box;
}

*,
*:before,
*:after {
  box-sizing: inherit;
}

body {
  font-size: 1.6rem;
  font-family: "Lato", sans-serif;
  background-color: var(--gris-claro);
}

h1 {
  font-size: 4rem;
}

h2 {
  font-size: 3rem;
}

header {
  background-color: var(--azul);
}

header h1 {
  padding: 3rem 0;
  margin: 0;
  color: var(--blanco);
  text-align: center;
}

.contenedor {
  width: 90%;
  max-width: 80rem;
  margin: 0 auto;
}

.contenedor-header {
  margin-top: -5rem;
  transform: translateY(5rem);
  padding: 5rem;
}

.dos-columnas {
  display: grid;
  gap: 4rem;
}

@media (min-width: 768px) {
  .dos-columnas {
    grid-template-columns: 2fr 1fr;
  }
}

.contenido-principal {
  display: flex;
  flex-direction: column;
  gap: 4rem;
}

.resumen {
  position: sticky;
  top: 2rem;
}

.tarjeta {
  padding: 2rem;
  border-radius: 1rem;
}

.tarjeta h2 {
  margin: 0 0 2rem 0;
  text-align: center;
  color: var(--gris-oscuro);
  font-size: 2.4rem;
}

.presupuesto-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 1.5rem;
}

.presupuesto-item p {
  font-size: 1.6rem;
  color: var(--gris-oscuro);
  margin: 0;
}

.presupuesto-item .precio {
  font-size: 1.8rem;
  font-weight: 700;
  color: var(--azul);
}

.presupuesto-item .negativo {
  color: #ef4444;
}

.sombra {
  box-shadow: 0px 10px 15px -3px rgba(0,0,0,0.1);
  background-color: var(--blanco);
  border-radius: 1.2rem;
  padding: 5rem;
}
</style>
