<script setup>
    import { formatearCantidad, formatearFecha } from '../helpers';
    
    import iconoAhorro from '../assets/icono_ahorro.svg';
    import iconoCasa from '../assets/icono_casa.svg';
    import iconoComida from '../assets/icono_comida.svg';
    import iconoGastos from '../assets/icono_gastos.svg';
    import iconoOcio from '../assets/icono_ocio.svg';
    import iconoSalud from '../assets/icono_salud.svg';
    import iconoSuscripciones from '../assets/icono_suscripciones.svg';
    
    const props = defineProps({
      gasto: {
        type: Object,
        required: true
      }
    });
    
    const emit = defineEmits(['seleccionar-gasto']);
    
    const diccionarioIconos = {
      ahorro: iconoAhorro,
      casa: iconoCasa,
      comida: iconoComida,
      gastos: iconoGastos,
      ocio: iconoOcio,
      salud: iconoSalud,
      suscripciones: iconoSuscripciones
    };
    </script>
    
    <template>
      <div class="gasto sombra">
        <div class="contenido">
          <img 
            :src="diccionarioIconos[gasto.categoria]" 
            alt="Icono gasto"
            class="icono"
          />
          <div class="detalles">
            <p class="categoria">{{ gasto.categoria }}</p>
            <p 
              class="nombre"
              @click="emit('seleccionar-gasto', gasto.id)"
            >
              {{ gasto.nombre }} (haz click para editar)
            </p>
            <p class="fecha">
              Fecha: <span>{{ formatearFecha(gasto.fecha) }}</span>
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
    }
    
    .contenido {
      display: flex;
      align-items: center;
      gap: 2rem;
    }
    
    .icono {
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
      cursor: pointer;
    }
    
    .fecha {
      color: var(--gris-oscuro);
      font-size: 1.6rem;
      font-weight: 900;
    }
    
    .fecha span {
      font-weight: 400;
    }
    
    .cantidad {
      font-size: 3rem;
      font-weight: 900;
      margin: 0;
    }
    </style>