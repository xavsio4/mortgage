<template>
  <img
    class="mx-auto"
    alt="Fifteenpeas mortage simulator"
    src="./assets/logo.png"
    width="60"
  />

  <div class="container mx-auto">
    <Mortgage :startprice="300000" />
  </div>
</template>

<script>
  //@ts-expect-error import HelloWorld from "./components/HelloWorld.vue";
  import Mortgage from "./components/Mortgage.vue";

  export default {
    name: "App",
    components: {
      //  HelloWorld,
      Mortgage,
    },
  };
</script>

<style>
  #app {
    text-align: center;

    margin-top: 60px;
  }
</style>
