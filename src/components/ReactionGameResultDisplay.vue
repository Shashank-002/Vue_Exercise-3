<template>
    <div class="message-box">
        {{ message }}
        <div v-if="reactionTime !== null" class="show-reaction-time">
            Your reaction time was {{ reactionTimeInSeconds }} seconds.
        </div>
    </div>
</template>

<script>
export default {
    name: 'ReactionGameResultDisplay',
    props: {
        message: String,
        reactionTime: Number,
    },
    computed: {
        reactionTimeInSeconds() {
            return this.reactionTime !== null ? (this.reactionTime / 1000).toFixed(3) : "0.000";
        },
    },
};
</script>

<style scoped>
.message-box {
    margin-top: 20px;
    background-color: #d9edf7;
    border-radius: 10px;
    font-size: 25px;
    text-align: left;
    color: #333;
    width: 430px;
    height: 120px;
    padding: 20px;
    gap: 10px;
}

.show-reaction-time {
    padding-top: 20px;
}

@media (max-width: 768px) {
    .message-box {
        font-size: 18px;
        width: 55%;
        height: 38%;
    }
}

@media (max-width: 575px) {
    .message-box {
        width: 50%;
    }
}


@media (max-width: 375px) {
    .message-box {
        border-radius: 5px;
    }
}

@media (min-width: 1600px) {
    .message-box {
        width: 50%;
        height: 25%;
        font-size: 40px;
        padding: 30px;
    }
}
</style>
