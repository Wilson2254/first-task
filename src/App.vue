<template>
  <div id="app">
    <FormMain />
  </div>
</template>

<script>
import FormMain from "./components/FormMain.vue";

export default {
  name: "App",
  data() {
    return {
    };
  },
  components: {
    FormMain,
  },
};
</script>

<style lang="scss">
* {
  font-family: "Vollkorn SC", serif;
  margin: 0;
  padding: 0;
}
</style>
