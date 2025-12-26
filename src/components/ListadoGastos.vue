<template>
  <div class="listado-gastos contenedor">
    <h2 v-if="gastos.length > 0">{{ titulo }}</h2>
    <div v-else class="no-gastos">
      <p>No hay gastos aún</p>
    </div>

    <div class="filtros">
      <select v-model="filtro" class="filtro">
        <option value="">-- Todas las categorías --</option>
        <option value="ahorro">Ahorro</option>
        <option value="comida">Comida</option>
        <option value="casa">Casa</option>
        <option value="gastos">Gastos Varios</option>
        <option value="ocio">Ocio</option>
        <option value="salud">Salud</option>
        <option value="suscripciones">Suscripciones</option>
      </select>
    </div>

    <ul class="gastos">
      <li v-for="gasto in gastosFiltrados" :key="gasto.id" class="gasto">
        <div class="contenido-gasto">
          <div class="imagen">
            <img :src="`/icono_${gasto.categoria}.svg`" :alt="gasto.categoria">
          </div>
          <div class="detalle">
            <p class="categoria">{{ gasto.categoria }}</p>
            <p class="nombre">{{ gasto.nombre }}</p>
            <p class="fecha">{{ formatearFecha(gasto.fecha) }}</p>
          </div>
        </div>
        <div class="acciones">
          <p class="cantidad">${{ gasto.cantidad }}</p>
          <button 
            class="btn-eliminar"
            @click="eliminarGasto(gasto.id)"
            title="Eliminar gasto"
          >
            <img src="/cerrar.svg" alt="Eliminar gasto">
          </button>
        </div>
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref, computed, inject } from 'vue';

const eliminarGasto = inject('eliminarGasto');

const gastos = inject('gastos');
const filtro = ref('');

const gastosFiltrados = computed(() => {
  if (filtro.value) {
    return gastos.value.filter(gasto => gasto.categoria === filtro.value);
  }
  return gastos.value;
});

const titulo = computed(() => {
  if (filtro.value) {
    return `Gastos de ${filtro.value}`;
  }
  return 'Todos los gastos';
});

const formatearFecha = (fecha) => {
  return new Date(fecha).toLocaleDateString('es-ES', {
    year: 'numeric',
    month: 'long',
    day: 'numeric'
  });
};
</script>

<style scoped>
.listado-gastos {
  margin-top: 10rem;
}

h2 {
  color: var(--gris-oscuro);
  font-weight: 700;
  font-size: 2.8rem;
  text-align: center;
  margin-bottom: 3rem;
}

.no-gastos {
  text-align: center;
  font-size: 2rem;
  color: var(--gris-oscuro);
}

.filtros {
  margin-bottom: 2rem;
}

.filtro {
  width: 100%;
  padding: 1rem;
  border: 1px solid var(--gris);
  border-radius: 1rem;
  font-size: 1.6rem;
  color: var(--gris-oscuro);
}

.gastos {
  margin-top: 2rem;
}

.gasto {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1.5rem;
  margin-bottom: 1.5rem;
  background-color: var(--blanco);
  border-radius: 0.8rem;
  box-shadow: 0 2px 4px rgba(0,0,0,0.05);
}

.contenido-gasto {
  display: flex;
  align-items: center;
  gap: 2rem;
}

.imagen {
  width: 8rem;
}

.imagen img {
  width: 100%;
}

.detalle p {
  margin: 0 0 0.5rem 0;
}

.categoria {
  color: var(--gris);
  font-size: 1.2rem;
  text-transform: uppercase;
  font-weight: 900;
}

.nombre {
  color: var(--gris-oscuro);
  font-size: 1.6rem;
  font-weight: 700;
}

.fecha {
  color: var(--gris);
  font-size: 1.4rem;
}

.acciones {
  display: flex;
  align-items: center;
  gap: 1rem;
}

.cantidad {
  font-size: 2rem;
  font-weight: 900;
  color: var(--azul);
  margin: 0;
}

.btn-eliminar {
  background: none;
  border: none;
  cursor: pointer;
  padding: 0.5rem;
}

.btn-eliminar img {
  width: 2rem;
  height: 2rem;
}

.btn-eliminar:hover {
  transform: scale(1.1);
}
</style>
