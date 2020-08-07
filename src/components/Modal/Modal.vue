<template>
  <div class="modal">
    <div class="header">
      <h2>Новый клиент успешно создан</h2>
    </div>
    <div class="info">
        <slot> </slot>
    </div>
    <div class="footer">
      <div class="close" @click="closeModal">ОК</div>
    </div>
  </div>
</template>

<script>
export default {
  methods: {
    closeModal() {
      this.$emit("closeModal");
    },
  },
};
</script>

<style lang="scss" scoped>
.modal {
    font-weight: 600;
  position: absolute;
  z-index: 1;
  height: 97%;
  width: 98%;
  opacity: 0.8;
}
</style>