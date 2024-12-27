<template>
    <div>
        <h1>Bienvenido a Games Opinion</h1>
        <div class="games-list">
            <GameCard v-for="(game, index) in games" :key="index" :game="game" @opinar="goToOpiniones(game.name)" />
        </div>
    </div>
</template>

<script>
import axios from 'axios';
import GameCard from '../components/GameCard.vue';

export default {
  name: 'HomeView',
  components: { GameCard },
  data() {
    return {
      games: [],
    };
  },
  created() {
    this.fetchGames();
  },
  methods: {
    async fetchGames() {
      try {
        const apiKey = 'd56a902fd098400f995e75a56a850746';
        const response = await axios.get(
          `https://api.rawg.io/api/games?key=${apiKey}`
        );
        this.games = response.data.results.map(game => ({
          id: game.id,
          name: game.name,
          image: game.background_image,
          rating: game.rating,
        }));
      } catch (error) {
        console.error('Error al obtener los juegos:', error);
      }
    },
    goToOpiniones(gameName) {
        this.$router.push(`/opiniones/${gameName}`);
    },
  },
};
</script>

<style scoped>
/* Contenedor de tarjetas de juegos */
.games-list {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
  justify-content: center;
  padding: 20px;
}
h1 {
  text-align: center;
  margin: 20px 0;
}
</style>