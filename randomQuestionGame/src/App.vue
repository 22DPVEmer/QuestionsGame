<template>
  <div id="app">
    <main>
      <TriviaGame />
    </main>
  </div>
</template>

<script>
import TriviaGame from "./components/icons/TriviaGame.vue";

export default {
  name: "App",
  components: {
    TriviaGame,
  },
};
</script>

<style scoped>
html,
body,
#app {
  height: 100%;
  margin: 0;
}

#app {
  display: flex;
  flex-direction: column;
}

main {
  flex: 1;
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
