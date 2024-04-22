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

    <!-- Popup pour afficher le message de victoire -->
    <div v-if="showWinPopup" class="popup">
      <div class="popup-content">
        <h2>{{ winMessage }}</h2>
      </div>
    </div>
  </div>
</template>

<script>
import { ref, onBeforeMount } from "vue";

export default {
  setup() {
    const showWinPopup = ref(false);
    const winMessage = ref("");

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
        currentCard.flipped = true;
        if (currentCard.content === "chat") {
          showWinPopup.value = true;
          winMessage.value = "Vous avez trouvé le chat!";
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
      showWinPopup.value = false;
      initializeGame(); // Replacer le chat sur une carte aléatoire
    };

    const restartGame = () => {
      showWinPopup.value = false;
      winMessage.value = "";
      // Ajoutez ici la logique pour réinitialiser le jeu selon vos besoins
    };

    // Fonction pour afficher la popup de victoire
    const showPopup = (message) => {
      winMessage.value = message;
      showWinPopup.value = true;
      setTimeout(hidePopup, 1000);
    };

    // Fonction pour masquer la popup de victoire
    const hidePopup = () => {
      showWinPopup.value = false;
      winMessage.value = "";
    };

    onBeforeMount(() => {
      initializeGame();
    });

    return {
      cards,
      handleCardClick,
      showWinPopup,
      winMessage,
      restartGame,
      showPopup,
    };
  },
};
</script>

<style>
body {
  justify-content: center;
  align-items: center;
  background: rgb(28, 229, 233);
  background: linear-gradient(
    90deg,
    rgba(28, 229, 233, 1) 0%,
    rgba(161, 195, 186, 1) 56%
  );
}

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
  border-radius: 10px;
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
  border-radius: 10px;
}

.card-back img {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}

.popup {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
}

.popup-content {
  padding: 40px;
  border-radius: 20px;
  text-align: center;
  color: white;
  animation: slideUpDown 1s ease forwards; /* Ajout de l'animation d'entrée */
}

.popup h2 {
  font-size: 3.5em; /* Taille de police agrandie */
  font-family: var(--header-font-family);
  text-shadow: 2px 2px 4px var(--text-border-color);
}

/* Définition de l'animation */
@keyframes slideUpDown {
  0% {
    transform: translateY(100%);
    opacity: 0;
  }
  50% {
    transform: translateY(0);
    opacity: 1;
  }
  100% {
    transform: translateY(0);
    opacity: 1;
  }
}
</style>
