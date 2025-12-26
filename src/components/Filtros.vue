<template>
  <div class="filtros sombra">
    <h2>Filtrar Gastos</h2>
    <div class="campo">
      <label for="filtro">Filtrar Gastos</label>
      <select 
        id="filtro"
        v-model="modelValue"
      >
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
  </div>
</template>

<script setup>
defineProps({
  modelValue: {
    type: String,
    required: true
  }
});

defineEmits(['update:modelValue']);
</script>

<style scoped>
.filtros {
  margin-top: 10rem;
}

.filtros h2 {
  color: var(--gris-oscuro);
}

.filtros .campo {
  display: flex;
  align-items: center;
  gap: 2rem;
}

.filtros select {
  flex: 1;
  padding: 1rem;
  border-radius: 1rem;
  border: none;
  background-color: var(--gris-claro);
}
</style>
