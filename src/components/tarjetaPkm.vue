<script setup lang="ts">
import { computed } from 'vue'
import { typeColors } from '@/assets/pokemonColors'

export interface datosPokemon {
  id: number
  name: string
  image: string
  types: string[]
  hp: number
  height: number
  weight: number
  moves: string[]
}

const props = defineProps<{
  pokemon: datosPokemon
}>()

// Tipo principal del Pokémon  
const tipoPrincipal = computed(() => {
  return props.pokemon.types?.[0]?.toLowerCase() ?? 'normal'
})

// Color de fondo de la carta 
const colorFondo = computed(() => {
  return typeColors[tipoPrincipal.value] ?? '#beb8af'
})

// Formateo de texto
const formatearNombre = (texto: string) => {
  if (!texto) return ''
  return texto.charAt(0).toUpperCase() + texto.slice(1).replace(/-/g, ' ')
}

const formatearId = (numeroId: number) => {
  return String(numeroId).padStart(4, '0')
}

// Altura: decímetros a metros 
const alturaFormateada = computed(() => {
  return (props.pokemon.height / 10).toFixed(1) + ' m'
})

// Peso: hectogramos a kilogramos 
const pesoFormateado = computed(() => {
  return (props.pokemon.weight / 10).toFixed(1) + ' kg'
})
</script>

<template>
  <article class="tarjeta-pokemon" :style="{ '--tipoBg': colorFondo }">
    <div class="tarjeta-cuerpo">
      <!-- Encabezado superior: Nombre a la izquierda y PS a la derecha -->
      <header class="tarjeta-header">
        <h2 class="nombre">{{ formatearNombre(pokemon.name) }}</h2>
        <div class="ps-contenedor">
          <span class="ps-label">PS</span>
          <span class="ps-valor">{{ pokemon.hp }}</span>
        </div>
      </header>

      <!--Ilustración central del Pokémon -->
      <div class="marco-imagen">
        <img
          :src="pokemon.image"
          :alt="pokemon.name"
          class="pokemon-imagen"
          loading="lazy"
        />
      </div>

      <!-- Cinta informativa con número y tipo -->
      <div class="cinta-info">
        <span>N.° {{ formatearId(pokemon.id) }} Pokémon tipo {{ formatearNombre(tipoPrincipal) }}</span>
      </div>

      <!--Sección de habilidades -->
      <section class="seccion-ataques">
        <div
          v-for="(habilidad, indice) in pokemon.moves.slice(0, 2)"
          :key="habilidad"
          class="ataque-item"
        >
          <span class="ataque-nombre">{{ formatearNombre(habilidad) }}</span>
          <span class="ataque-dano">{{ (indice + 1) * 20 }}</span>
        </div>
      </section>

      <!-- 4. Franja inferior: Altura y Peso -->
      <footer class="franja-datos">
        <div class="dato-bloque">
          <span class="dato-etiqueta">Altura</span>
          <span class="dato-valor">{{ alturaFormateada }}</span>
        </div>
        <div class="divisor-vertical"></div>
        <div class="dato-bloque">
          <span class="dato-etiqueta">Peso</span>
          <span class="dato-valor">{{ pesoFormateado }}</span>
        </div>
      </footer>
    </div>
  </article>
</template>

<style scoped src="@/assets/tarjetaPkm.css"></style>
