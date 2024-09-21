<template>
    <div class="reaction-game" :class="backgroundClass">
        <button :class="buttonClass" @click="handleButtonClick">
            {{ buttonText }}
        </button>
        <div class="message-box">
            {{ message }}
            <div v-if="reactionTime !== null" class="ShowreactionTime">
                Your reaction time was {{ reactionTimeInSeconds }} seconds.
            </div>
        </div>

        <!-- Popup Modal -->
        <div v-if="showModal" class="PopUpmodal">
            <div class="modal-content">
                <div class="error-icon">
                    <i class="info-icon">i</i>
                </div>
                <h2>Winner!</h2>
                <p>Your score of {{ reactionTimeHourFormat }} has been stored in your browser and could be lost when you
                    leave. Create an account or login to automatically save them to your account!</p>
                <button @click="closeModal" class="got-it-btn">Got it!</button>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'ReactionGamePlay',
    data() {
        return {
            buttonText: "Go",
            message: "Click Go to test your reaction time!",
            gameStarted: false,
            buttonClass: "start-button",
            backgroundClass: "",
            timeoutID: null,
            gameCanStop: false,
            startTime: null,
            reactionTime: null,
            showModal: false,
        };
    },
    computed: {
        reactionTimeInSeconds() {
            return this.reactionTime !== null ? (this.reactionTime / 1000).toFixed(3) : "0.000";
        },
        reactionTimeHourFormat() {
            if (this.reactionTime === null) {
                return "00:00:00.000";
            }

            const date = new Date(this.reactionTime);
            const hours = date.getUTCHours();
            const minutes = date.getUTCMinutes();
            const seconds = date.getUTCSeconds();
            const milliseconds = date.getUTCMilliseconds();

            return `${String(hours).padStart(2, '0')}:${String(minutes).padStart(2, '0')}:${String(seconds).padStart(2, '0')}.${String(milliseconds).padStart(3, '0')}`;
        },
    },
    methods: {
        handleButtonClick() {
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
            this.buttonClass = "stop-button";
            this.reactionTime = null;
            const delay = Math.floor(Math.random() * (5000 - 2000 + 1)) + 2000;

            this.timeoutID = setTimeout(() => {
                this.gameCanStop = true;
                this.startTime = performance.now();
                this.backgroundClass = "ready";
            }, delay);
        },
        stopGame() {
            if (this.gameCanStop) {
                this.reactionTime = performance.now() - this.startTime;
                this.message = "Click Go to test your reaction time again!";
                this.saveReactionTime(this.reactionTime);
                this.showModal = true;
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
            this.$emit('savedScore', time);
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
            this.backgroundClass = "";
            this.buttonClass = "start-button";
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
.reaction-game {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    height: 60vh;
    max-width: 80%;
    margin: 10px auto;
    border-radius: 10px;
    background-color: #3498db;
}

.start-button,
.stop-button {
    width: 53%;
    height: 25%;
    font-size: 40px;
    border: none;
    border-radius: 15px;
    color: white;
    cursor: pointer;
}

.start-button {
    background-color: #5cb85c;
}

.stop-button {
    background-color: #d9534f;
}

.message-box {
    margin-top: 20px;
    background-color: #d9edf7;
    border-radius: 10px;
    font-size: 25px;
    text-align: left;
    color: #333;
    width: 50%;
    height: 25%;
    padding: 20px;
    gap: 10px;
}

.ShowreactionTime {
    padding-top: 20px;
}

.reaction-game.ready {
    background-color: #5cb85c;
}

.PopUpmodal {
    position: fixed;
    z-index: 1;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.4);
    display: flex;
    justify-content: center;
    align-items: center;
}

.got-it-btn {
    margin-top: 20px;
    padding: 10px 20px;
    background-color: #6c63ff;
    color: white;
    border: none;
    border-radius: 5px;
    cursor: pointer;
}

.modal-content {
    background-color: white;
    padding: 20px;
    border-radius: 8px;
    width: 400px;
    text-align: center;
    position: relative;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    opacity: 0;
    transform: scale(0.8);
    animation: pop-in 0.5s ease forwards;
}

.error-icon {
    position: relative;
    font-size: 40px;
    color: #00bfff;
    margin: 20px 0;
    width: 80px;
    height: 80px;
    border: 4px solid #00bfff;
    border-radius: 50%;
    display: flex;
    justify-content: center;
    align-items: center;
    animation: swing 1s ease-in-out;
}

.info-icon {
    font-size: 40px;
    color: #00bfff;
}

@keyframes swing {
    0% {
        transform: rotate(0deg);
    }

    25% {
        transform: rotate(15deg);
    }

    50% {
        transform: rotate(0deg);
    }

    75% {
        transform: rotate(-15deg);
    }

    100% {
        transform: rotate(0deg);
    }
}

@keyframes pop-in {
    0% {
        opacity: 0;
        transform: scale(0.8);
    }

    100% {
        opacity: 1;
        transform: scale(1);
    }
}

@media (max-width:1024px) {
    .message-box {
        margin-top: 20px;
        background-color: #d9edf7;
        border-radius: 10px;
        font-size: 18px;
        text-align: left;
        color: #333;
        width: 60%;
        height: 25%;
        padding: 20px;
        gap: 10px;
    }

    .start-button,
    .stop-button {
        width: 65%;
        height: 25%;
        font-size: 40px;
        border: none;
        border-radius: 15px;
        color: white;
        cursor: pointer;
    }

}

@media (max-width:768px) {
    .message-box {
        margin-top: 20px;
        background-color: #d9edf7;
        border-radius: 10px;
        font-size: 18px;
        text-align: left;
        color: #333;
        width: 66%;
        height: 25%;
        padding: 20px;
        gap: 10px;
    }

    .start-button,
    .stop-button {
        width: 73%;
        height: 25%;
        font-size: 40px;
        border: none;
        border-radius: 15px;
        color: white;
        cursor: pointer;
    }

}

@media (max-width:575px) {
    .message-box {
        margin-top: 20px;
        background-color: #d9edf7;
        border-radius: 10px;
        font-size: 18px;
        text-align: left;
        color: #333;
        width: 50%;
        height: 38%;
        padding: 20px;
        gap: 10px;
    }

    .start-button,
    .stop-button {
        width: 60%;
        height: 25%;
        font-size: 40px;
        border: none;
        border-radius: 15px;
        color: white;
        cursor: pointer;
    }

    .modal-content {
        width: 280px;
    }

}

@media (max-width:425px) {
    .message-box {
        margin-top: 20px;
        background-color: #d9edf7;
        border-radius: 10px;
        font-size: 18px;
        text-align: left;
        color: #333;
        width: 50%;
        height: 38%;
        padding: 20px;
        gap: 10px;
    }

    .start-button,
    .stop-button {
        width: 65%;
        height: 25%;
        font-size: 40px;
        border: none;
        border-radius: 15px;
        color: white;
        cursor: pointer;
    }

    .modal-content {
        width: 230px;
    }

}

@media (max-width:375px) {
    .message-box {
        margin-top: 20px;
        background-color: #d9edf7;
        border-radius: 10px;
        font-size: 18px;
        text-align: left;
        color: #333;
        width: 50%;
        height: 38%;
        padding: 20px;
        gap: 10px;
    }

    .start-button,
    .stop-button {
        width: 65%;
        height: 25%;
        font-size: 40px;
        border: none;
        border-radius: 15px;
        color: white;
        cursor: pointer;
    }

    .modal-content {
        width: 220px;
    }
}

@media (min-width:1600px) {
    .message-box {
        margin-top: 20px;
        background-color: #d9edf7;
        border-radius: 10px;
        font-size: 40px;
        text-align: left;
        color: #333;
        width: 50%;
        height: 25%;
        padding: 30px;
    }

    .start-button,
    .stop-button {
        width: 53%;
        height: 25%;
        font-size: 80px;
        border: none;
        border-radius: 15px;
        color: white;
        cursor: pointer;
    }

    .modal-content {
        width: 500px;
        height: 400px;
    }
}
</style>