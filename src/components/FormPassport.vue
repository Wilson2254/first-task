<template>
  <div>
    <div>
      Тип документа*
      <select>
        <option>Паспорт</option>
        <option>Свидетельство о рождении</option>
        <option>Вод. удостоверение</option>
      </select>
    </div>
    <div>
      Серия
      <input type="text" placeholder="Серия" />
    </div>
    <div>
      Номер
      <input type="text" placeholder="Номер" />
    </div>
    <div>
      Кем выдан
      <input placeholder="Кем выдан" type="text" />
    </div>
    <div>
      Дата выдачи*
      <input type="date" placeholder="Улица"  v-model="dateGive" @input="$v.dateObject.$touch"/>
      <div
          class="error"
          v-if="!$v.dateObject.required && $v.dateObject.$dirty || $v.dateObject.$error"
        >Некорректная дата</div>
    </div>
    <div>
      Дом
      <input type="text" placeholder="Дом" />
    </div>
  </div>
</template>

<script>
import { required, maxValue, minValue } from "vuelidate/lib/validators";
export default {
  data() {
    return {
      series: "",
      num: "",
      whoGive: "",
      dateGive:
        new Date().getFullYear() +
        "-" +
        String(new Date().getMonth() + 1).padStart(2, "0") +
        "-" +
        String(new Date().getDate()).padStart(2, "0"),
      house: "",
    };
  },
  validations: {
    series: {},
    num: {},
    dateGive: {},
    dateObject: {
      required,
      maxValue: maxValue(new Date()),
      minValue: minValue(new Date("1900-01-01")),
    },
    house: {},
  },

  computed: {
    dateObject() {
      return this.dateGive ? new Date(this.dateGive) : null;
    },
  },
};
</script>

<style lang="scss" scoped>
</style>