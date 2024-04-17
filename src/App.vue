<template>
  <div class="container">
    <div
      v-for="(card, index) in cards"
      :key="index"
      @click="handleCardClick(index)"
      class="card"
    >
      <div v-if="!card.flipped" class="card-front">
        <!-- Affiche le contenu de la carte à l'avant -->
        ?
      </div>
      <div v-else class="card-back">
        <!-- Affiche le contenu de la carte à l'arrière -->
        <img v-if="card.content === 'chat'" src="./assets/cat.png" alt="Chat" />
        <div v-else>{{ card.content }}</div>
      </div>
    </div>
  </div>
</template>

<script>
import { ref, onBeforeMount } from "vue";

export default {
  setup() {
    const cards = ref([
      { content: "X", flipped: false },
      { content: "X", flipped: false },
      { content: "X", flipped: false },
      { content: "X", flipped: false },
      { content: "X", flipped: false },
      { content: "X", flipped: false },
      { content: "X", flipped: false },
      { content: "X", flipped: false },
      { content: "X", flipped: false },
    ]);
    let clickCount = ref(0);
    const maxClicks = 3;

    const handleCardClick = (index) => {
      const currentCard = cards.value[index];
      if (!currentCard.flipped) {
        currentCard.flipped = true; // Inverser la carte
        if (currentCard.content === "chat") {
          alert("Vous avez trouvé le chat!");
          setTimeout(resetGame, 800);
        } else {
          clickCount.value++;
          if (clickCount.value === maxClicks) {
            setTimeout(resetGame, 800);
          }
        }
      }
    };

    const initializeGame = () => {
      const chatIndex = Math.floor(Math.random() * cards.value.length);
      cards.value[chatIndex].content = "chat";
    };

    // Dans la méthode resetGame
    const resetGame = () => {
      clickCount.value = 0;
      cards.value.forEach((card) => {
        card.flipped = false;
        card.content = "X"; // Réinitialiser le contenu de toutes les cartes à 'X'
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
html,
body {
  height: 100%;
  margin: 0;
  padding: 0;
}

body {
  display: flex;
  justify-content: center;
  align-items: center;
  background: rgb(28, 229, 233);
  background: linear-gradient(
    90deg,
    rgba(28, 229, 233, 1) 0%,
    rgba(161, 195, 186, 1) 56%
  );
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
  background-color: #eadeda; /* Or */
  min-height: 250px;
  width: 250px;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 24px;
}

.card-back {
  position: relative; /* Définit la position relative pour pouvoir positionner l'image à l'intérieur */

  background-color: #6495ed; /* Bleu cornflower */
  min-height: 250px;
  width: 250px;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 24px;
}

.card-back img {
  position: absolute; /* Positionne l'image par rapport au conteneur parent */
  top: 0;
  left: 0;
  width: 100%; /* Utilisez la largeur complète de la carte */
  height: 100%; /* Utilisez la hauteur complète de la carte */
}
</style>
