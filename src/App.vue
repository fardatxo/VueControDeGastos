<script setup>
  import { ref, reactive, watch, computed, onMounted } from 'vue';
  import Presupuesto from './components/Presupuesto.vue';
  import ControlPresupuesto from './components/ControlPresupuesto.vue';
  import Modal from './components/Modal.vue';
  import Gasto from './components/Gasto.vue';
  import Filtro from './components/Filtro.vue';
  import { generarId } from './helpers';
  // import iconoNuevoGasto from './assets/nuevo-gasto.svg'; // Comentado temporalmente
  
  const presupuesto = ref(0);
  const disponible = ref(0);
  const gastado = ref(0);
  const filtro = ref('');
  
  const modal = reactive({
    mostrar: false,
    animar: false
  });
  
  const gasto = reactive({
    nombre: '',
    cantidad: '',
    categoria: '',
    id: null,
    fecha: Date.now()
  });
  
  const gastos = ref([]);
  
  // Watch para calcular gastado y disponible
  watch(gastos, () => {
    const totalGastado = gastos.value.reduce((total, gasto) => total + gasto.cantidad, 0);
    gastado.value = totalGastado;
    disponible.value = presupuesto.value - totalGastado;
  
    // Guardar en localStorage
    localStorage.setItem('gastos', JSON.stringify(gastos.value));
  }, {
    deep: true
  });
  
  // Watch para guardar presupuesto en localStorage
  watch(presupuesto, () => {
    localStorage.setItem('presupuesto', presupuesto.value);
  });
  
  // Watch para reiniciar gasto cuando se cierra el modal
  watch(modal, () => {
    if (!modal.mostrar) {
      reiniciarStateGasto();
    }
  }, {
    deep: true
  });
  
  // Computed para gastos filtrados
  const gastosFiltrados = computed(() => {
    if (filtro.value) {
      return gastos.value.filter(gasto => gasto.categoria === filtro.value);
    }
    return gastos.value;
  });
  
  // Definir presupuesto
  const definirPresupuesto = (cantidad) => {
    presupuesto.value = cantidad;
    disponible.value = cantidad;
  };
  
  // Mostrar modal
  const mostrarModal = () => {
    modal.mostrar = true;
    setTimeout(() => {
      modal.animar = true;
    }, 500);
  };
  
  // Ocultar modal
  const ocultarModal = () => {
    modal.animar = false;
    setTimeout(() => {
      modal.mostrar = false;
    }, 500);
  };
  
  // Guardar gasto
  const guardarGasto = () => {
    if (gasto.id) {
      // Editar gasto existente
      const index = gastos.value.findIndex(g => g.id === gasto.id);
      gastos.value[index] = { ...gasto };
    } else {
      // Añadir nuevo gasto
      gastos.value.push({
        ...gasto,
        id: generarId(),
        fecha: Date.now()
      });
    }
  
    ocultarModal();
    reiniciarStateGasto();
  };
  
  // Reiniciar state gasto
  const reiniciarStateGasto = () => {
    Object.assign(gasto, {
      nombre: '',
      cantidad: '',
      categoria: '',
      id: null,
      fecha: Date.now()
    });
  };
  
  // Seleccionar gasto para editar
  const seleccionarGasto = (id) => {
    const gastoEditar = gastos.value.find(g => g.id === id);
    Object.assign(gasto, gastoEditar);
    mostrarModal();
  };
  
  // Eliminar gasto
  const eliminarGasto = () => {
    if (confirm('¿Estás seguro de eliminar este gasto?')) {
      gastos.value = gastos.value.filter(g => g.id !== gasto.id);
      ocultarModal();
    }
  };
  
  // Resetear app
  const resetearApp = () => {
    if (confirm('¿Deseas resetear presupuesto y gastos?')) {
      gastos.value = [];
      presupuesto.value = 0;
      disponible.value = 0;
      localStorage.removeItem('presupuesto');
      localStorage.removeItem('gastos');
    }
  };
  
  // Recuperar datos de localStorage al montar
  onMounted(() => {
    const presupuestoStorage = localStorage.getItem('presupuesto');
    if (presupuestoStorage) {
      presupuesto.value = Number(presupuestoStorage);
      disponible.value = Number(presupuestoStorage);
    }
  
    const gastosStorage = localStorage.getItem('gastos');
    if (gastosStorage) {
      gastos.value = JSON.parse(gastosStorage);
    }
  });
  </script>
  
  <template>
    <div :class="{fijar: modal.mostrar}">
      <header>
        <h1>Planificador de Gastos</h1>
  
        <div class="contenedor-header contenedor sombra">
          <Presupuesto 
            v-if="presupuesto === 0"
            @definir-presupuesto="definirPresupuesto"
          />
  
          <ControlPresupuesto
            v-else
            :presupuesto="presupuesto"
            :disponible="disponible"
            :gastado="gastado"
            @reset-app="resetearApp"
          />
        </div>
      </header>
  
      <main v-if="presupuesto > 0">
        <Filtro 
          v-model:filtro="filtro"
        />
  
        <div class="listado-gastos contenedor">
          <h2>{{ gastos.length > 0 ? 'Gastos' : 'No hay gastos aún' }}</h2>
  
          <Gasto 
            v-for="gasto in gastosFiltrados"
            :key="gasto.id"
            :gasto="gasto"
            @seleccionar-gasto="seleccionarGasto"
          />
        </div>
  
        <div class="crear-gasto">
          <!-- <img 
            :src="iconoNuevoGasto"
            alt="icono nuevo gasto"
            @click="mostrarModal"
          /> -->
          <button 
            @click="mostrarModal"
            style="width: 5rem; height: 5rem; border-radius: 50%; background: #3b82f6; color: white; border: none; font-size: 3rem; cursor: pointer;"
          >
            +
          </button>
        </div>
  
        <Modal 
          v-if="modal.mostrar"
          :modal="modal"
          v-model:nombre="gasto.nombre"
          v-model:cantidad="gasto.cantidad"
          v-model:categoria="gasto.categoria"
          :disponible="disponible"
          :id="gasto.id"
          @ocultar-modal="ocultarModal"
          @guardar-gasto="guardarGasto"
          @eliminar-gasto="eliminarGasto"
        />
      </main>
    </div>
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
  
  .sombra {
    box-shadow: 0px 10px 15px -3px rgba(0,0,0,0.1);
    background-color: var(--blanco);
    border-radius: 1.2rem;
    padding: 5rem;
  }
  
  .crear-gasto {
    position: fixed;
    bottom: 5rem;
    right: 5rem;
  }
  
  .crear-gasto img {
    width: 5rem;
    cursor: pointer;
  }
  
  .listado-gastos {
    margin-top: 10rem;
  }
  
  .listado-gastos h2 {
    font-weight: 900;
    color: var(--gris-oscuro);
  }
  
  .fijar {
    overflow: hidden;
    height: 100vh;
  }
  </style>