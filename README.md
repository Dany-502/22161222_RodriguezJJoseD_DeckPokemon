# Pokédex Clase · Deck de 40 cartas Pokémon

Aplicación hecha con Vue.js 3 que arma un deck de 40 cartas Pokémon aleatorias y las muestra todas en una sola vista.

Todos los datos son obtenidos mediante [PokeAPI](https://pokeapi.co/).

## Autor

- **Nombre:** Rodríguez Juárez José Daniel
- **Número de control:** 22161222
- **Institución:** Instituto Tecnológico de Oaxaca
- **Materia:** Desarrollo de Servicios Web

## Características

- Deck de 40 Pokémon aleatorios y sin repetir.
- Botón "Obtener nuevo mazo" para barajar otra vez.
- Cada carta muestra:
  - Nombre y HP (Puntos de Salud)
  - ID de la Pokédex
  - Tipo(s), con el color de fondo de la carta según el tipo principal
  - Habilidades/ataques con su respectivo daño
  - Franja de datos con altura y peso (en metros y kilogramos)

## Tecnologías

- [Vue.js 3](https://vuejs.org/) con `<script setup>` y TypeScript
- [Vite](https://vite.dev/)
- [Vue Router](https://router.vuejs.org/)
- [PokeAPI](https://pokeapi.co/)

## Instalación y uso

Requisitos: [Node.js](https://nodejs.org/) 22.18 o superior y conexión a internet (para consultar la PokeAPI).

1. Clonar el repositorio e instalar las dependencias:
```bash
npm install
```

2. Iniciar el servidor de desarrollo:
```bash
npm run dev
```

3. Abrir en el navegador: [http://localhost:5173](http://localhost:5173)


