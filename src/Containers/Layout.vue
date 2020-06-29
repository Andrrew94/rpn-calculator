<template>
  <div class="layout">
    <Header />
    <Calculator />
    <Footer />
  </div>
</template>
<script>
  import Calculator from "../components/Calculator";
  import Header from "../components/Header";
  import Footer from "../components/Footer";
  export default {
    name: 'Layout',
    components: {Footer, Header, Calculator}
  }
</script>

<style lang="scss">
  .layout {
    width: 100vw;
    height: 100vh;
  }
</style>
