<template>
  <div class="formulario-gasto contenedor sombra">
    <form @submit.prevent="handleAgregarGasto">
      <legend>Añade un gasto</legend>

      <div class="campo">
        <label for="nombre">Nombre Gasto:</label>
        <input 
          type="text" 
          id="nombre" 
          placeholder="Añade el nombre del gasto" 
          v-model="nombre"
          required
        />
      </div>

      <div class="campo">
        <label for="cantidad">Cantidad:</label>
        <input 
          type="number" 
          id="cantidad" 
          placeholder="Añade la cantidad del gasto" 
          min="0"
          v-model.number="cantidad"
          required
        />
      </div>

      <div class="campo">
        <label for="categoria">Categoría:</label>
        <select id="categoria" v-model="categoria" required>
          <option value="">-- Seleccione --</option>
          <option value="ahorro">Ahorro</option>
          <option value="comida">Comida</option>
          <option value="casa">Casa</option>
          <option value="gastos">Gastos Varios</option>
          <option value="ocio">Ocio</option>
          <option value="salud">Salud</option>
          <option value="suscripciones">Suscripciones</option>
        </select>
      </div>

      <input type="submit" value="Añadir Gasto" />
    </form>
  </div>
</template>

<script setup>
import { ref, inject } from 'vue';

const nombre = ref('');
const cantidad = ref(0);
const categoria = ref('');
const gastos = inject('gastos');

const agregarGasto = inject('agregarGasto');

const handleAgregarGasto = () => {
  const gasto = {
    id: Date.now(),
    nombre: nombre.value,
    cantidad: cantidad.value,
    categoria: categoria.value,
    fecha: new Date().toISOString()
  };

  agregarGasto(gasto);
  
  // Reset form
  nombre.value = '';
  cantidad.value = 0;
  categoria.value = '';
};
</script>

<style scoped>
.formulario-gasto {
  margin-top: 2rem;
}

legend {
  font-size: 2rem;
  color: var(--azul);
  text-align: center;
  margin-bottom: 2rem;
  font-weight: 700;
}

.campo {
  display: grid;
  gap: 1rem;
  margin-bottom: 2rem;
}

.campo label {
  font-size: 1.8rem;
  color: var(--gris-oscuro);
}

.campo input,
.campo select {
  padding: 1rem;
  border: 1px solid var(--gris);
  border-radius: 1rem;
  font-size: 1.6rem;
}

input[type="submit"] {
  background-color: var(--azul);
  border: none;
  padding: 1rem;
  font-size: 2rem;
  text-align: center;
  margin-top: 2rem;
  color: var(--blanco);
  font-weight: 900;
  width: 100%;
  transition: background-color 300ms ease;
}

input[type="submit"]:hover {
  background-color: #1048A4;
  cursor: pointer;
}
</style>
