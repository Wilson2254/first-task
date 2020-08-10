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
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  z-index: 10;
  height: 100%;
  opacity: 0.9;
  .footer{
      .close{
          background: gray;
          cursor: pointer;
          width: 150px;
          text-align: center;
          border: 2px solid;
          opacity: 1;
          margin-top: 20px;
          &:hover{
              opacity: 0.7;
              color: white;
          }
      }
  }
  .header{
    h2{
      margin-top: 80px;
      text-align: center;
    }
  }
  .info{
    width: 300px;
    border: 2px solid;
    text-align: center;
    background: white;
  }
}
</style>