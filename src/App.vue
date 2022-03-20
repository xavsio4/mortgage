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
  <p class="my-6">
    get the code at
    <a target="_blank" href="https://github.com/xavsio4/mortgage"
      >https://github.com/xavsio4/mortgage</a
    >
  </p>
  Get it on your phone
  <img
    class="mx-auto mb-6"
    alt="Fifteenpeas mortage simulator"
    src="./assets/qrcode_mortgage.png"
    width="100"
  />
  A fifteenpeas labs sunday project
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
