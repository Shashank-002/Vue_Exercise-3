<template>
    <div class="message-box">
        {{ message }}
        <div v-if="reactionTime !== null" class="ShowreactionTime">
            Your reaction time was {{ reactionTimeInSeconds }} seconds.
        </div>
    </div>
</template>

<script>
export default {
    name: 'ShowResult',
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

.ShowreactionTime {
    padding-top: 20px;
}

@media (max-width:768px) {
    .message-box {
        margin-top: 20px;
        background-color: #d9edf7;
        border-radius: 10px;
        font-size: 18px;
        text-align: left;
        color: #333;
        width: 55%;
        height: 38%;
        padding: 20px;
        gap: 10px;
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
}
</style>
