<template>
  <div id="app">
      <HelloWorld />
      <FormAddress />
      <FormMain />
      <FormPassport />
  </div>
</template>

<script>
import HelloWorld from "./components/HelloWorld.vue";
import FormAddress from "./components/FormAddress.vue";
import FormMain from "./components/FormMain.vue";
import FormPassport from "./components/FormPassport.vue";

export default {
  name: "App",
  data() {
    return {
      showModal: false,
    };
  },
  components: {
    HelloWorld,
    FormAddress,
    FormMain,
    FormPassport,
  },

};
</script>

<style lang="scss">
</style>
