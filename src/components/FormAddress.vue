<template>
  <div>
    <div>
      Индекс
      <input placeholder="000000" type="text" maxlength="10" />
    </div>
    <div>
      Страна
      <input type="text" placeholder="Страна" />
    </div>
    <div>
      Область
      <input type="text" placeholder="Область" />
    </div>
    <div>
      Город*
      <input placeholder="Город" type="text" v-model.trim="$v.city.$model" />
      <div class="error" v-if="!$v.city.required && $v.city.$dirty">Введите город</div>
    </div>
    <div>
      Улица
      <input type="text" placeholder="Улица" />
    </div>
    <div>
      Дом
      <input type="text" placeholder="Дом" />
    </div>
  </div>
</template>

<script>
import { required } from "vuelidate/lib/validators";
export default {
  data() {
    return {
      city: "",
    };
  },
  validations: {
    city: { required },
  },
};
</script>

<style lang="scss" scoped>
</style>