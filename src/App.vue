<template>
  <div id="app">
    <FormMain />
    <FormAddress />
    <FormPassport />
  </div>
</template>

<script>
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
    FormAddress,
    FormMain,
    FormPassport,
  },
};
</script>

<style lang="scss">
</style>
