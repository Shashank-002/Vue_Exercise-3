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
                <tr v-for="(time, index) in sortedTimes" :key="index">
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
    name: 'ReactionGameResult',
    data() {
        return {
            reactionTimes: [],
        };
    },
    computed: {
        sortedTimes() {
            // Sort the times in ascending order (fastest first)
            const sorted = [...this.reactionTimes].sort((a, b) => a.score - b.score);
            return sorted.slice(0, 10);
        },
    },
    methods: {
        formatTime(score) {
            const date = new Date(score);
            const hours = date.getUTCHours();
            const minutes = date.getUTCMinutes();
            const seconds = date.getUTCSeconds();
            const milliseconds = date.getUTCMilliseconds();

            const formattedHours = String(hours).padStart(2, '0');
            const formattedMinutes = String(minutes).padStart(2, '0');
            const formattedSeconds = String(seconds).padStart(2, '0');
            const formattedMilliseconds = String(milliseconds).padStart(3, '0');

            // Return the formatted time as hh:mm:ss.mmm
            return `${formattedHours}:${formattedMinutes}:${formattedSeconds}.${formattedMilliseconds}`;
        },

        // Fetch scores from localStorage
        fetchScores() {
            const savedScores = JSON.parse(localStorage.getItem('reactionTimes')) || [];

            // Sort scores and store only the top 10 in the array
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