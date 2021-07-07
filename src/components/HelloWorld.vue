<template>
  <div class="tama">
    <div class="imageContainer">
      <img :src="imageSrc" />
    </div>
    <div class="buttonDiv">
      <button @click="send('POO')">Poo</button>
      <button @click="send('CLEAN')">Clean</button>
      <button @click="send('YAWN')">Yawn</button>
      <button @click="send('RUMBLE')">RUMBLE</button>
      <button @click="send('FEED')">Feed</button>
      <button @click="send('ROCK')">Rock</button>
      <button @click="send('IGNORED')">Ignore</button>
      <button @click="send('LOUDNOISE')">Loud Noise</button>
      <button @click="send('MORNING')">Morning</button>
    </div>
  </div>
</template>

<script>
import { createMachine } from "xstate";
import { useMachine } from "@xstate/vue";

const babyMachine = createMachine({
  id: "baby",
  initial: "happy",
  states: {
    sleeping: {
      on: {
        LOUDNOISE: "crying",
        MORNING: "happy",
      },
    },
    hungry: {
      on: {
        FEED: "happy",
      },
    },
    fulldiaper: {
      on: {
        CLEAN: "happy",
      },
    },
    tired: {
      on: {
        ROCK: "sleeping",
        IGNORED: "crying",
      },
    },
    happy: {
      on: {
        POO: "fulldiaper",
        YAWN: "tired",
        RUMBLE: "hungry",
        LOUDNOISE: "fulldiaper",
      },
    },
    crying: {
      on: {
        ROCK: "happy",
      },
    },
  },
});

export default {
  name: "HelloWorld",
  setup() {
    const { state, send } = useMachine(babyMachine);
    return {
      state,
      send,
    };
  },
  computed: {
    imageSrc() {
      return `/images/${this.state.value}.jpg`;
    },
  },
  mounted() {
    setTimeout(() => {
      console.log("Sending a yawn event");
      this.send("YAWN");
    }, 10000);
    setTimeout(() => {
      console.log("Sedning a loud noise")
      this.send("LOUDNOISE")
    }, 10000 * Math.random())
  },
};
</script>

<style scoped>
img {
  display: block;
  width: 300px;
  margin: auto;
}

.imageContainer {
  padding-top: 150px;
}

.tama {
  margin: 0 auto;
  background-image: url("/images/tama.png");
  width: 400px;
  height: 600px;
  background-size: 400px;
  background-repeat: no-repeat;
}

.buttonDiv {
  margin-top: 300px;
}
</style>
