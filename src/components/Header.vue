<template>
  <div class="header">
    <div class="header__title">Reverse polish notation calculator
      <a href="https://github.com/Andrrew94/rpn-calculator" target="_blank">
        <img src="../assets/images/github.png" alt="github">
      </a>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'Header'
  }
</script>

<style lang="scss" scoped>
  .header {
    text-align: center;
    padding: 30px;

    &__title {
      font-size: 2em;
      position: relative;

      a {
        position: absolute;
        width: 25px;
        margin-left: 10px;

        img {
          max-width: 100%;
        }
      }
    }
  }
</style>
