<template>
  <div class="container">
    <div v-for="(card, index) in cards" :key="index"  @click="handleCardClick(index)">
      <div class="card-front"></div>
      <vue-flip :options="flipOptions">
        <template v-slot:front>
          <!-- Contenu de la carte côté face -->
          <div class="card-front"></div>
        </template>
        <template v-slot:back>
          <!-- Contenu de la carte côté verso -->
          <div class="card-back">{{ card === 'chat' ? '🐱' : '' }}</div>
        </template>
      </vue-flip>
    </div>
  </div>
</template>

<script>
import { ref } from 'vue';
import { VueFlip } from 'vue-flip';

export default {
  components: {
    // VueFlip,
    'vue-flip': VueFlip
  },
  setup() {
    const cards = ref(['vide', 'vide', 'vide', 'vide', 'vide', 'vide', 'vide', 'vide', 'vide']);
    let clickCount = ref(0);
    const maxClicks = 3;
    const flipOptions = {
      duration: 500, // Durée de l'animation de retournement (en ms)
    };

    const handleCardClick = (index) => {
      if (cards.value[index] === 'chat') {
        alert('Vous avez trouvé le chat!');
        // Réinitialiser le jeu
        resetGame();
      } else {
        cards.value[index] = 'vide'; // Marquer la carte comme vide
        clickCount.value++;
        if (clickCount.value === maxClicks) {
          // Si le joueur a cliqué trois fois sans trouver le chat, déplacez le chat
          const newIndex = Math.floor(Math.random() * cards.value.length);
          cards.value[newIndex] = 'chat';
          // Réinitialiser le jeu
          resetGame();
        }
      }
    };

    const resetGame = () => {
      // Réinitialiser le jeu après une victoire ou si le joueur n'a pas trouvé le chat
      clickCount.value = 0;
      // Réinitialiser toutes les cartes à vide
      cards.value = ['vide', 'vide', 'vide', 'vide', 'vide', 'vide', 'vide', 'vide', 'vide'];
    };

    return {
      cards,
      flipOptions,
      handleCardClick,
    };
  },
};
</script>

<style>
/* Styles pour la grille et les cartes */
.container {
  display: grid;
  grid-template-columns: repeat(3, 1fr); /* Utilisation de fractions pour que chaque colonne occupe la même largeur */
  background-color: red;
  grid-gap: 5px;
}

.card-front, .card-back {
  background-color: yellow;
  height: 200px;
  width: 200px;
  border: 2px solid green;
  justify-content: center; /* Centrer horizontalement */
  font-size: 24px;
}



/* Ajoutez des styles supplémentaires pour personnaliser l'apparence */
</style>
