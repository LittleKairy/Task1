<template>
  <div id="app">
    <div class="task">
      <h3>第一题</h3>
      <Container />
    </div>
    <div class="task">
      <h3>第二题</h3>
      <IconHover />
    </div>
  </div>
</template>

<script>
import Container from "@/components/Container";
import IconHover from "@/components/IconHover";
export default {
  name: "App",
  components: {
    Container,
    IconHover,
  },
};
</script>

<style lang="less" scoped>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

.task {
  width: 100vw;
  margin: 80px 0;
}
.task:first-child {
  margin-top: 0;
}
</style>
