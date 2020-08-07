<template>
  <div class="modal">
    <div class="header">
      <h2>Новый клиент успешно создан</h2>
    </div>
    <div class="info">
      <slot></slot>
    </div>
    <div class="footer">
      <div class="close" @click="closeModal">Закрыть</div>
    </div>
  </div>
</template>

<script>
export default {
  methods: {
    // Обращаюсь к родительскому элементу для заркытия модального окна
    closeModal() {
      this.$emit("closeModal");
    },
  },
};
</script>

<style lang="scss" scoped>
// Вывод модального окна поверх основного
.modal {
  font-weight: 600;
  position: absolute;
  z-index: 10;
  height: 100%;
  opacity: 0.8;
  .footer{
      .close{
          background: darkolivegreen;;
          cursor: pointer;
          width: 150px;
          text-align: center;
          border: 2px solid;
          opacity: 1;
          &:hover{
              opacity: 0.7;
          }
      }
  }
}
</style>