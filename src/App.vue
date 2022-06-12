<script setup>
import Box from "./components/SingleBox.vue";
</script>

<template>
  <div class="wrapper">
    <Box
      v-for="(eachBox, index) in box"
      :key="index"
      :index="index"
      :box="eachBox"
      @changeBoxColor="changeBoxColor"
    />
  </div>
</template>
<script>
export default {
  name: "App",
  components: {
    Box,
  },
  data() {
    return {
      box: [],
      windowWidth: window.innerWidth,
      getBoxes: 0,
    };
  },
  watch: {
    windowWidth: {
      immediate: true,
      handler() {
        this.getBoxes = Math.floor(this.windowWidth / 168);
      },
    },
  },
  unmounted() {
    window.removeEventListener("scroll", this.handleScroll);
  },
  mounted() {
    window.addEventListener("scroll", this.handleScroll);
    window.addEventListener("resize", this.onResize);

    this.generateBox();
  },

  methods: {
    onResize() {
      this.windowWidth = window.innerWidth;
    },
    generateBox() {
      for (let i = 0; i < 25; i++) {
        this.box.push(this.generateRandomColor());
      }
    },
    handleScroll() {
      if (window.innerHeight + window.scrollY >= document.body.offsetHeight) {
        this.generateBox();
      }
    },
    generateRandomColor() {
      const letters = "0123456789ABCDEF";
      let color = "#";
      for (let i = 0; i < 6; i++) {
        color += letters[Math.floor(Math.random() * 16)];
      }
      return color;
    },
    changeBoxColor(index) {
      this.box[index] = this.generateRandomColor();
      this.findAdjacentBox(index);
    },
    findAdjacentBox(index) {
      if (this.getBoxes <= 1) {
        console.log("No Adjacent box found");
      } else if (this.getBoxes - (index % this.getBoxes) > 1) {
        this.box[index + 1] = this.generateRandomColor();
      } else {
        this.box[index - 1] = this.generateRandomColor();
      }
    },
  },
};
</script>

<style>
@import "./assets/base.css";
#app {
  max-width: 1280px;
  margin: 0 auto;
  padding: 2rem;
  font-weight: normal;
}

.wrapper {
  display: flex;
  flex-wrap: wrap;
  justify-content: left;
  align-items: stretch;
}
</style>
