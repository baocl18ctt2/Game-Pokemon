<template>
  <div
    id="main-screen"
    class="screen"
  >
    <h1>poke memories</h1>
    <h3>Select mode to start game</h3>
    <div class="m-actions">
      <button
        v-for="(value, name) in modePlay"
        :key='name'
        @click="onStart(name)"
      >
        <span>{{ name }}</span>
        <span>{{ value }}</span>
      </button>
    </div>
  </div>
  <!-- Phần component register -->
  <register
    v-if="isForm"
    @onStartGame='onStartGame'
  />
</template>

<script>
import Register from "../components/FormInformation.vue";
export default {
  el: "#main-screen",
  components: {
    Register,
  },
  data() {
    return {
      modePlay: {
        "2x2": "Very Easy",
        "4x4": "Easy",
        "6x6": "Normal",
        "8x8": "Hard",
        "10x10": "Super Hard",
      },
      isForm: false,
      name: null,
    };
  },
  methods: {
    onStart(name) {
      this.name = name;
      this.isForm = true;
      // this.$emit("onStart", { totalOfBlock: name });
    },
    onStartGame() {
      this.$emit("onStart", { totalOfBlock: this.name });
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
.m-actions {
  display: flex;
  margin-top: 2rem;
}
.m-actions button {
  font: var(--font);
  width: 150px;
  height: 150px;
  background: transparent;
  box-shadow: none;
  border: 1px solid #fff;
  color: #fff;
  display: flex;
  flex-direction: column;
  border-radius: 1rem;
  margin: 0 1rem;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  transition: background 0.3s ease-in-out;
}
.m-actions button:hover {
  background-color: var(--light);
  color: var(--dark);
}
.m-actions button span:first-child {
  font-size: 2rem;
}
.m-actions button span:last-child {
  display: block;
  font-size: 1.25rem;
  margin-top: 0.5rem;
}
</style>