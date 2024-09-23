<template>
    <div>
        <button :class="reactionGameButton" @click="onStartStopButtonClick">
            {{ buttonText }}
        </button>

        <!-- Popup Modal -->
        <div v-if="showModal" class="reactionGameModal">
            <div class="modalInnerContent">
                <div class="error-icon">
                    <i class="info-icon">i</i>
                </div>
                <h2>Winner!</h2>
                <p>{{ modalMessage }}</p>
                <button @click="closeModal" class="confirmation-button">Got it!</button>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'ReactionGameStartStopButton',
    props: {
        buttonText: {
            type: String,
            required: true,
        },
        reactionGameButton: {
            type: String,
            required: true,
        },
        onButtonClick: {
            type: Function,
            required: true,
        },
        showModal: {
            type: Boolean,
            required: true,
        },
        modalMessage: {
            type: String,
            default: "",
        },
    },
    methods: {
        onStartStopButtonClick() {
            this.onButtonClick();
        },
        closeModal() {
            this.$emit('closeModal');
        },
    },
};
</script>

<style scoped>
.start-button,
.stop-button {
    width: 465px;
    height: 100px;
    font-size: 40px;
    border: none;
    border-radius: 10px;
    color: white;
    cursor: pointer;
}

.start-button {
    background-color: #5cb85c;
}

.stop-button {
    background-color: #d9534f;
}

.reaction-game.ready {
    background-color: #5cb85c;
}

.reactionGameModal {
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

.confirmation-button {
    margin-top: 20px;
    padding: 10px 20px;
    background-color: #6c63ff;
    color: white;
    border: none;
    border-radius: 5px;
    cursor: pointer;
}

.modalInnerContent {
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

    .start-button,
    .stop-button {
        width: 469px;
        height: 90px;
        font-size: 40px;
    }

}

@media (max-width: 768px) {

    .start-button,
    .stop-button {
        width: 370px;
        height: 75px;
        font-size: 32px;
    }
}

@media (max-width: 575px) {

    .start-button,
    .stop-button {
        width: 203px;
        height: 70px;
        font-size: 30px;
    }

    .modalInnerContent {
        width: 280px;
    }
}

@media (max-width: 425px) {

    .start-button,
    .stop-button {
        width: 180px;
        height: 50px;
        font-size: 20px;
    }

    .modalInnerContent {
        width: 220px;
    }
}


@media (max-width: 320px) {

    .start-button,
    .stop-button {
        width: 160px;
    }

    .modalInnerContent {
        width: 220px;
    }
}

@media (min-width:1600px) {

    .start-button,
    .stop-button {
        width: 946px;
        height: 160px;
        font-size: 60px;
    }

    .modalInnerContent {
        width: 500px;
        height: 400px;
    }

    h2 {
        font-size: 30px;
    }

    p {
        font-size: 20px;
    }
}
</style>
