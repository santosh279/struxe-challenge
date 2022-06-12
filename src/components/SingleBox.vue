<template>
  <div class="wrapper">
    <div
      class="box"
      :style="{ 'background-color': box }"
      @click="$emit('changeBoxColor', index)"
    ></div>
  </div>
</template>
<script>
export default {
  props: {
    box: {},
    index: {},
  },
};
</script>
<style scoped>
.box {
  width: 140px;
  height: 140px;
  border: 2px solid black;
  transition: background-color 2s ease;
}
</style>
