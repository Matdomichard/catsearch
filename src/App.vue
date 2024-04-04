<template>
  <div class="container">
    <div v-for="(card, index) in cards" :key="index" @click="handleCardClick(index)" class="card">
      <div v-if="!card.flipped" class="card-front"> <!-- Affiche le contenu de la carte à l'avant -->
        Front
      </div>
      <div v-else class="card-back"> <!-- Affiche le contenu de la carte à l'arrière -->
        {{ card.content }}
      </div>
    </div>
  </div>
</template>

<script>
import { ref } from 'vue';

export default {
  setup() {
    const cards = ref([
      { content: '', flipped: false },
      { content: '', flipped: false },
      { content: '', flipped: false },
      { content: '', flipped: false },
      { content: '', flipped: false },
      { content: '', flipped: false },
      { content: '', flipped: false },
      { content: '', flipped: false },
      { content: '', flipped: false },
    ]);
    let clickCount = ref(0);
    const maxClicks = 3;

    const handleCardClick = (index) => {
      const currentCard = cards.value[index];
      if (!currentCard.flipped) {
        if (currentCard.content === 'chat') {
          alert('Vous avez trouvé le chat!');
          resetGame();
        } else {
          currentCard.flipped = true;
          clickCount.value++;
          if (clickCount.value === maxClicks) {
            const newIndex = Math.floor(Math.random() * cards.value.length);
            cards.value[newIndex].content = 'chat';
            resetGame();
          }
        }
      }
    };

    const resetGame = () => {
      clickCount.value = 0;
      cards.value.forEach(card => {
        card.flipped = false;
        card.content = '';
      });
    };

    return {
      cards,
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
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 24px;
}
</style>
