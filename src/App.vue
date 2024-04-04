<template>
  <div class="container">
    <div v-for="(card, index) in cards" :key="index" @click="handleCardClick(index)" class="card">
      <div v-if="!card.flipped" class="card-front"> <!-- Affiche le contenu de la carte à l'avant -->
        ?
      </div>
      <div v-else class="card-back"> <!-- Affiche le contenu de la carte à l'arrière -->
        {{ card.content }}
      </div>
    </div>
  </div>
</template>

<script>
import { ref, onBeforeMount } from 'vue';

export default {
  setup() {
    const cards = ref([
      { content: 'X', flipped: false },
      { content: 'X', flipped: false },
      { content: 'X', flipped: false },
      { content: 'X', flipped: false },
      { content: 'X', flipped: false },
      { content: 'X', flipped: false },
      { content: 'X', flipped: false },
      { content: 'X', flipped: false },
      { content: 'X', flipped: false },
    ]);
    let clickCount = ref(0);
    const maxClicks = 3;

    const handleCardClick = (index) => {
  const currentCard = cards.value[index];
  if (!currentCard.flipped) {
    currentCard.flipped = true; // Inverser la carte
    if (currentCard.content === 'chat') {
      alert('Vous avez trouvé le chat!');
      resetGame();
    } else {
      clickCount.value++;
      if (clickCount.value === maxClicks) {
        const newIndex = Math.floor(Math.random() * cards.value.length);
        cards.value[newIndex].content = 'chat';
        cards.value[newIndex].flipped = false; // Assurez-vous que la nouvelle carte contenant le chat n'est pas retournée
        resetGame();
      }
    }
  }
};


const initializeGame = () => {
  const chatIndex = Math.floor(Math.random() * cards.value.length);
  cards.value[chatIndex].content = 'chat';
};

// Dans la méthode resetGame
const resetGame = () => {
  clickCount.value = 0;
  cards.value.forEach(card => {
    card.flipped = false;
    card.content = 'X'; // Réinitialiser le contenu de toutes les cartes à 'X'
  });
  initializeGame(); // Replacer le chat sur une carte aléatoire
};

onBeforeMount(() => {
      initializeGame();
    });

    return {
      cards,
      handleCardClick,
    };
  },
};
</script>

<style>

html, body {
  height: 100%; 
  margin: 0; 
  padding: 0; 
}

body {
  display: flex; 
  justify-content: center; 
  align-items: center; 
}
/* Styles pour la grille et les cartes */
.container {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  width: 800px;
  grid-gap: 5px;
  margin: auto;
}

.card-front {
  background-color: #FFD700; /* Or */
  min-height: 250px;
  width: 250px;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 24px;
}

.card-back {
  background-color: #6495ED; /* Bleu cornflower */
  min-height: 250px;
  width: 250px;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 24px;
}

</style>
