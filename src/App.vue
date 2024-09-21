<template>
  <div>
    <ReactionGamePlay />
    <ReactionGameResult />
  </div>
</template>

<script>
import ReactionGamePlay from './components/ReactionGamePlay.vue';
import ReactionGameResult from './components/ReactionGameResult.vue';

export default {
  name: 'App',
  components: {
    ReactionGamePlay,
    ReactionGameResult,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
