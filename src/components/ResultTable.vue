<template>
    <div class="high-scores">
        <h2>Your High Scores</h2>
        <p v-if="!reactionTimes.length">No high scores yet. Try playing a game!</p>
        <table v-else>
            <thead>
                <tr>
                    <th>Rank</th>
                    <th>Date</th>
                    <th>Score</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="(time, index) in sortedReactionTimes" :key="index">
                    <td>{{ index + 1 }}</td>
                    <td>{{ time.date }}</td>
                    <td>{{ formatTime(time.score) }}</td>
                </tr>
            </tbody>
        </table>
    </div>
</template>


<script>
export default {
    name: 'ReactionTable',
    data() {
        return {
            reactionTimes: JSON.parse(localStorage.getItem("reactionTimes")) || [],
        };
    },
    computed: {
        sortedReactionTimes() {
            const sorted = [...this.reactionTimes].sort((a, b) => a.score - b.score);
            return sorted.slice(0, 10);
        },
    },
    methods: {
        formatTime(score) {
            const date = new Date(score);
            return `${String(date.getUTCHours()).padStart(2, '0')}:${String(date.getUTCMinutes()).padStart(2, '0')}:${String(date.getUTCSeconds()).padStart(2, '0')}.${String(date.getUTCMilliseconds()).padStart(3, '0')}`;
        },

        // Fetch scores from localStorage
        fetchScores() {
            const savedScores = JSON.parse(localStorage.getItem('reactionTimes')) || [];

            this.reactionTimes = savedScores
                .sort((a, b) => a.score - b.score)
                .slice(0, 10);
        },

        // Save the new score and ensure only top 10 scores are stored
        saveScore(newScore) {
            const savedScores = JSON.parse(localStorage.getItem('reactionTimes')) || [];

            // Add the new score
            savedScores.push({
                score: newScore,
                date: new Date().toLocaleString(),
            });

            // Sort the scores in ascending order and store the top 10
            const topScores = savedScores.sort((a, b) => a.score - b.score).slice(0, 10);

            // Save the top 10 scores back to localStorage
            localStorage.setItem('reactionTimes', JSON.stringify(topScores));
            this.reactionTimes = topScores;
        },
    },
    mounted() {
        this.fetchScores();
    },
};
</script>


<style scoped>
.high-scores {
    max-width: 80%;
    margin: 20px auto;
}

table {
    width: 100%;
    border-collapse: collapse;
}

th,
td {
    padding: 8px;
    text-align: left;
    border-bottom: 1px solid #ddd;
}

thead {
    background-color: #f4f4f4;
}

h2 {
    font-size: 22px;
    margin-bottom: 20px;
    margin-left: 20px;
    text-align: left;
}

p {
    text-align: left;
    margin-left: 20px;
}

@media (max-width:1024px) {
    h2 {
        font-size: 20px;
        margin-bottom: 20px;
    }
}

@media (max-width:768px) {
    h2 {
        font-size: 20px;
        margin-bottom: 20px;
    }
}

@media (max-width:575px) {
    td {
        padding: 8px;
        text-align: left;
        border-bottom: 1px solid #ddd;
        font-size: 14px;
    }

    h2 {
        font-size: 18px;
        margin-bottom: 20px;
    }
}

@media (max-width:425px) {
    td {
        padding: 8px;
        text-align: left;
        border-bottom: 1px solid #ddd;
        font-size: 12px;
    }

    h2 {
        font-size: 18px;
        margin-bottom: 20px;
    }
}

@media (max-width:375px) {
    td {
        padding: 8px;
        text-align: left;
        border-bottom: 1px solid #ddd;
        font-size: 12px;
    }

    h2 {
        font-size: 18px;
        margin-bottom: 20px;
    }
}

@media (min-width:1600px) {

    .high-scores {
        max-width: 70%;
        margin: 20px auto;
    }

    tr,
    td {
        padding: 8px;
        text-align: left;
        border-bottom: 1px solid #ddd;
        font-size: 30px;
    }

    h2 {
        font-size: 32px;
        margin-bottom: 30px;
    }
}
</style>