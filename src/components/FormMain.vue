<template>
  <div>
    <form @submit.prevent="submitForm">
      <div>
        Фамилия*
        <input placeholder="Фамилия" v-model.trim="$v.secondName.$model" />
        <div class="error" v-if="!$v.secondName.required && $v.secondName.$dirty">Введите фамилию</div>
      </div>
      <div>
        Имя*
        <input placeholder="Имя" v-model.trim="$v.firstName.$model" />
        <div class="error" v-if="!$v.firstName.required && $v.firstName.$dirty">Введите имя</div>
      </div>
      <div>
        Отчество
        <input placeholder="Отчество" />
      </div>
      <div>
        Дата рождения*
        <input
          type="date"
          v-model="birthDate"
          @input="$v.dateObject.$touch"
        />
        <div
          class="error"
          v-if="!$v.dateObject.required && $v.dateObject.$dirty || $v.dateObject.$error"
        >Некорректная дата</div>
      </div>
      <p>
        Номер телефона
        <input placeholder="Номер телефона" type="tel" v-model="phone"/>
      </p>
      <p>
        Пол
        <input placeholder="Пол" />
      </p>Группа клиентов*
      <select multiple>
        <option value="volvo">VIP</option>
        <option value="saab">Проблемные</option>
        <option value="opel">ОМС</option>
      </select>
      <p>
        Лечащий врач
        <select>
          <option>Иванов</option>
          <option>Захаров</option>
          <option>Чернышева</option>
        </select>
      </p>Не отправлять СМС
      <input type="checkbox" />
      <button type="submit">Отправить</button>
    </form>
  </div>
</template>

<script>
import { required, maxValue, minValue } from "vuelidate/lib/validators";
const isPhone = (value) => /^1(3|4|5|7|8)\d{9}$/.test(value);  //phone valid
export default {
  data() {
    return {
      secondName: "",
      firstName: "",
      thirdName: "", //Отчество
      birthDate:
        new Date().getFullYear() +
        "-" +
        String(new Date().getMonth() + 1).padStart(2, "0") +
        "-" +
        String(new Date().getDate()).padStart(2, "0"),
      phone: "",
      gender: "",
      group: "",
      doctor: "",
      sendSMS: false,
    };
  },
  validations: {
    secondName: { required },
    firstName: { required },
    thirdName: {}, //Отчество
    dateObject: {
      required,
      maxValue: maxValue(new Date()),
      minValue: minValue(new Date("1900-01-01")),
    },
    phone: { required, phoneValid:isPhone },
    gender: {},
    group: { required },
    doctor: {},
    sendSMS: false,
  },
  methods: {
    submitForm() {
      if (this.$v.$invalid) {
        this.$v.$touch();
        return;
      }
      alert("Все норм");
    },
  },
  computed: {
    dateObject() {
      return this.birthDate ? new Date(this.birthDate) : null;
    },
  },
};
</script>

<style scoped lang="scss">
.error {
  color: red;
}
.valid {
  color: green;
}

input {
}
::-webkit-calendar-picker-indicator {
  display: none;
}
::-webkit-inner-spin-button {
  display: none;
}
</style>>
