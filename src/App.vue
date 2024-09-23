<template>
  <div>
    <div :class="['app', backgroundClass]">
      <ReactionGameStartStopButton :buttonText="buttonText" :reactionGameButton="reactionGameButton"
        :onButtonClick="onStartStopButtonClick" :showModal="showModal" :modalMessage="modalMessage"
        @closeModal="closeModal" />
      <ReactionGameResultDisplay :message="message" :reactionTime="reactionTime" @savedScore="updateScore" />
    </div>
    <ReactionGameTopScoreTable ref="highScoresComponent" />
  </div>

</template>

<script>
import ReactionGameStartStopButton from './components/ReactionGameStartStopButton.vue';
import ReactionGameResultDisplay from './components/ReactionGameResultDisplay.vue';
import ReactionGameTopScoreTable from './components/ReactionGameTopScoreTable.vue'

export default {
  name: 'App',
  components: {
    ReactionGameStartStopButton,
    ReactionGameResultDisplay,
    ReactionGameTopScoreTable,
  },
  data() {
    return {
      buttonText: "Go",
      message: "Click Go to test your reaction time!",
      gameStarted: false,
      reactionGameButton: "start-button",
      backgroundClass: "initial-bg",
      timeoutID: null,
      gameCanStop: false,
      startTime: null,
      reactionTime: null,
      showModal: false,
      modalMessage: "",
    };
  },
  methods: {
    updateScore() {
      this.$refs.highScoresComponent.fetchScores();
    },
    onStartStopButtonClick() {
      if (!this.gameStarted) {
        this.startGame();
      } else if (this.gameCanStop) {
        this.stopGame();
      } else {
        this.showError();
      }
    },
    startGame() {
      this.buttonText = "Stop";
      this.message = "Pay attention. Click stop when the color changes.";
      this.gameStarted = true;
      this.gameCanStop = false;
      this.reactionGameButton = "stop-button";
      this.backgroundClass = "initial-bg";
      this.reactionTime = null;

      const delay = Math.floor(Math.random() * (5000 - 2000 + 1)) + 2000;
      this.timeoutID = setTimeout(() => {
        this.gameCanStop = true;
        this.startTime = performance.now();
        this.backgroundClass = "success-bg";
      }, delay);
    },
    stopGame() {
      if (this.gameCanStop) {
        this.reactionTime = performance.now() - this.startTime;
        const totalSeconds = Math.floor(this.reactionTime / 1000);
        const hours = Math.floor(totalSeconds / 3600);
        const minutes = Math.floor((totalSeconds % 3600) / 60);
        const seconds = (this.reactionTime / 1000).toFixed(3);

        const formattedTime = `${hours.toString().padStart(2, '0')}:${minutes.toString().padStart(2, '0')}:${seconds.toString().padStart(2, '0')}`;
        this.modalMessage = `Your score of ${(formattedTime)} has been stored in your browser and could be lost when you leave. Create an account or login to automatically save them to your account!`;
        this.showModal = true;
        this.message = "Click Go to test your reaction time again!";
        this.saveReactionTime(this.reactionTime);
      } else {
        this.showError();
      }
      this.resetGame();
    },
    saveReactionTime(time) {
      const reactionTimes = JSON.parse(localStorage.getItem('reactionTimes')) || [];
      reactionTimes.push({
        score: time,
        date: new Date().toLocaleString(),
      });
      localStorage.setItem('reactionTimes', JSON.stringify(reactionTimes));
      this.updateScore();
    },
    showError() {
      this.message = "Too quick... Try again!";
      this.reactionTime = null;
      this.resetGame();
    },
    resetGame() {
      clearTimeout(this.timeoutID);
      this.buttonText = "Go";
      this.gameStarted = false;
      this.gameCanStop = false;
      this.backgroundClass = "initial-bg";
      this.reactionGameButton = "start-button";
    },
    closeModal() {
      this.showModal = false;
    },
  },
  beforeUnmount() {
    clearTimeout(this.timeoutID);
  },
};
</script>

<style scoped>
.app {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 60vh;
  transition: background-color 0.5s ease;
  margin: 50px auto;
  max-width: 80%;
  border-radius: 10px;
}

.initial-bg {
  background-color: #3498db;
}

.success-bg {
  background-color: #5cb85c;
}

@media(min-width:1600px) {
  .app {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    height: 60vh;
    transition: background-color 0.5s ease;
    margin: 50px auto;
    max-width: 70%;
    border-radius: 10px;
  }
}
</style>
