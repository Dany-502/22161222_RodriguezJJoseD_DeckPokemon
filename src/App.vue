<script setup lang="ts">
import TarjetaPkm, { type datosPokemon } from './components/tarjetaPkm.vue'
import { ref, onMounted } from 'vue'

const idsPkmn = ref<number[]>([])
const listaPokemons = ref<datosPokemon[]>([])
const cargando = ref(true)

// Funcion para generar 40 numeros aleatorios entre 1 y 1025
function generarIdsAleatorios() {
  idsPkmn.value = []
  for (let i = 0; i < 40; i++) {
    const id = Math.floor(Math.random() * 1025) + 1
    // Verifica si el número ya existe
    if (!idsPkmn.value.includes(id)) {
      idsPkmn.value.push(id)
    } else {
      i--
    }
  }
  return idsPkmn.value
}

// Funcion para cargar el mazo de las 40 cartas
async function cargarMazo() {
  cargando.value = true
  generarIdsAleatorios()

  try {
    const datosPkm = await Promise.all(
      idsPkmn.value.map(async (id) => {
        const res = await fetch(`https://pokeapi.co/api/v2/pokemon/${id}`)
        const datos = await res.json()
        return {
          id: datos.id,
          name: datos.name,
          image:
            datos.sprites.other?.['official-artwork']?.front_default ??
            datos.sprites.front_default,
          types: datos.types.map((t: any) => t.type.name),
          hp: datos.stats.find((s: any) => s.stat.name === 'hp')?.base_stat ?? 50,
          height: datos.height,
          weight: datos.weight,
          moves: datos.moves.map((a: any) => a.move.name),
        }
      })
    )

    listaPokemons.value = datosPkm
  } catch (error) {
    console.error('error al cargar el mazo', error)
  } finally {
    cargando.value = false
  }
}

onMounted(() => {
  cargarMazo()
})
</script>

<template>
  <main class="contenedorAplicacion">
    <header class="encabezado">
      <h1 class="titulo">Mazo Pokémon</h1>
      <p class="subtitulo">Colección de 40 cartas aleatorias de la PokéAPI</p>
      <button class="botonRecargar" :disabled="cargando" @click="cargarMazo">
        {{ cargando ? 'Cargando cartas...' : 'Obtener nuevo mazo' }}
      </button>
    </header>

    <!-- Estado de carga mientras descarga de la PokéAPI -->
    <div v-if="cargando" class="estadoCarga">
      <div class="spinner"></div>
      <p>Descargando las 40 cartas de la PokéAPI...</p>
    </div>

    <!-- Cuadrícula con las 40 tarjetas -->
    <div v-else class="contenedorCartas">
      <TarjetaPkm
        v-for="pokemon in listaPokemons"
        :key="pokemon.id"
        :pokemon="pokemon"
      />
    </div>
  </main>
</template>

<style scoped>
.contenedorAplicacion {
  max-width: 1440px;
  margin: 0 auto;
  padding: 2.5rem 1.5rem 4rem 1.5rem;
  box-sizing: border-box;
}

.encabezado {
  text-align: center;
  margin-bottom: 2.5rem;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.6rem;
}

.titulo {
  margin: 0;
  font-size: 2.6rem;
  font-weight: 900;
  color: #ffffff;
  letter-spacing: -0.5px;
  text-shadow: 0 4px 12px rgba(0, 0, 0, 0.4);
}

.subtitulo {
  margin: 0;
  font-size: 1.1rem;
  color: #9ca3af;
}

.botonRecargar {
  margin-top: 0.5rem;
  padding: 0.65rem 1.4rem;
  font-size: 0.95rem;
  font-weight: 600;
  color: #ffffff;
  background-color: #dc2626; 
  border: none;
  border-radius: 6px;
  cursor: pointer;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);   
  transition: background-color 0.2s ease;
}
.botonRecargar:hover:not(:disabled) {
  background-color: #b91c1c;
}

.botonRecargar:disabled {
  opacity: 0.6;
  cursor: not-allowed;
  transform: none;
}

/* Spinner y mensaje de carga */
.estadoCarga {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  min-height: 380px;
  gap: 1.25rem;
  color: #e5e7eb;
  font-size: 1.2rem;
  font-weight: 600;
}

.spinner {
  width: 50px;
  height: 50px;
  border: 5px solid rgba(255, 255, 255, 0.15);
  border-top-color: #ef4444;
  border-radius: 50%;
  animation: girar 0.8s linear infinite;
}

@keyframes girar {
  to {
    transform: rotate(360deg);
  }
}

/* Cuadrícula que organiza las 40 tarjetas */
.contenedorCartas {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(290px, 1fr));
  gap: 2rem;
  justify-items: center;
}
</style>
