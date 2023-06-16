<template>
  <main-screen v-if="statusMatch === 'default'" @start="start($event)" />
  <interact-screen
    @finish="getResult"
    v-if="statusMatch === 'inMatch'"
    :cardsContext="setting.cardsContext"
  />
  <result-screen
    :time="time"
    v-if="statusMatch === 'Result'"
    @startAgain="statusMatch = 'default'"
  />
</template>

<script>
import MainScreen from "./components/MainScreen.vue";
import InteractScreen from "./components/InteractScreen.vue";
import ResultScreen from "./components/ResultSceen.vue";

export default {
  components: {
    MainScreen,
    InteractScreen,
    ResultScreen,
  },
  name: "App",
  data() {
    return {
      setting: {
        block: 0,
        cardsContext: [],
        startedAt: null,
      },
      statusMatch: "default",
      time: 0,
    };
  },
  methods: {
    start(config) {
      console.log("total block:", config.block);
      this.setting.block = config.block;
      let firstCards = Array.from(
        { length: this.setting.block / 2 },
        (_, i) => i + 1
      );
      let secondCards = [...firstCards];
      let mixCards = firstCards.concat(secondCards);
      for (let index = 0; index < 4; index++) {
        this.setting.cardsContext = mixCards.sort(() => Math.random() - 0.5);
      }
      this.setting.startedAt = new Date().getTime();
      this.statusMatch = "inMatch";
    },
    getResult() {
      this.time = new Date().getTime() - this.setting.startedAt;
      this.statusMatch = "Result";
    },
  },
};
</script>

<style lang="css" scoped>
.screen {
  width: 100%;
  height: 100vh;
  position: absolute;
  top: 0;
  left: 0;
  z-index: 2;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  background-color: var(--dark);
  color: var(--light);
}

.screen h1 {
  font-size: 4.5rem;
  text-transform: uppercase;
}

.screen p {
  font-size: 2rem;
}

.modes {
  display: flex;
  margin-top: 2rem;
}

.modes button {
  font: var(--font);
  width: 150px;
  height: 150px;
  background: transparent;
  box-shadow: none;
  border: 1px solid var(--light);
  color: var(--light);
  display: flex;
  flex-direction: column;
  border-radius: 1rem;
  margin: 0 1rem;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  transition: background 0.3s ease-in-out;
}

.modes button:hover {
  background-color: var(--light);
  color: var(--dark);
}

.modes button span:first-child {
  font-size: 2rem;
}

.modes button span:last-child {
  display: block;
  font-size: 1.25rem;
  margin-top: 0.5rem;
}
</style>
