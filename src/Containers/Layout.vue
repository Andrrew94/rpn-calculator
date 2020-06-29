<template>
  <div class="layout">
    <Header />
    <Calculator />
  </div>
</template>
<script>
  import Calculator from "../components/Calculator";
  import Header from "../components/Header";
  export default {
    name: 'Layout',
    components: {Header, Calculator}
  }
</script>

<style lang="scss">
  .layout {
    width: 100vw;
    height: 100vh;
    background-color: #fafafa;
  }
</style>
