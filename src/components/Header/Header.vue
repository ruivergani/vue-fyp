<template>
  <header class="header">
    <div class="container">
      <div>
        <h1 class="title">Vue.js</h1>
        <p class="subtitle">Algorithm to calculate DOM Operations and display benchmark results</p>
      </div>
      <div class="navigation">
        <!-- Menu -->
        <Menu :onButtonClick="onButtonClick" />
      </div>
    </div>
  </header>
</template>

<script>
  import Menu from '../Menu/Menu.vue';

  export default {
    components: {
      Menu,
    },
    props: {
      onButtonClick: {
        type: Function,
        required: true,
      },
    },
  };
</script>

<style src="./Header.css" scoped></style>