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
        <input placeholder="Отчество" v-model.trim="$v.thirdName.$model" />
      </div>
      <div>
        Дата рождения*
        <input type="date" v-model="birthDate" @input="$v.dateObject.$touch" />
        <div
          class="error"
          v-if="!$v.dateObject.required && $v.dateObject.$dirty || $v.dateObject.$error"
        >Некорректная дата</div>
      </div>
      <div>
        Номер телефона
        <input
          placeholder="Номер телефона"
          type=" text"
          v-model="phone"
          @input="acceptNumber"
        />
      </div>
      <div
        class="error"
        v-if="this.phone.length != 14 && $v.phone.$dirty"
      >Укажите правильный номер телефона</div>
      <p>
        Пол
        <input placeholder="Пол" />
      </p>
      <div>
        Группа клиентов*
        <select multiple @mousedown="chooseClient" v-model="group">
          <option value="VIP">VIP</option>
          <option value="Проблемные">Проблемные</option>
          <option value="ОМС">ОМС</option>
        </select>
      </div>
      <div
        class="error"
        v-if="this.group.length == 0 && !$v.group.required && $v.group.$dirty"
      >Балбес?</div>
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
      phone: "7",
      group: [],
    };
  },
  validations: {
    secondName: { required },
    firstName: { required },
    thirdName: {},
    dateObject: {
      required,
      maxValue: maxValue(new Date()),
      minValue: minValue(new Date("1900-01-01")),
    },
    phone: { required },
    group: { required },
  },
  methods: {
    submitForm() {
      if (this.$v.$invalid) {
        this.$v.$touch();
        console.log(this.phone.length);
        return;
      } else {
        alert("Все норм");
      }
    },
    acceptNumber() {
      let x = this.phone
        .replace(/\D/g, "")
        .match(/(\d{0,1})(\d{0,3})(\d{0,3})(\d{0,4})/);
      if (x[1] !== 7) x[1] = 7;
      this.phone = !x[2]
        ? x[1]
        : x[1] +
          "-" +
          x[2] +
          (x[3] ? "-" + x[3] : "") +
          (x[4] ? "-" + x[4] : "");
    },
    chooseClient(e) {
      var el = e.target;
      if (
        el.tagName.toLowerCase() == "option" &&
        el.parentNode.hasAttribute("multiple")
      ) {
        e.preventDefault();
        if (el.hasAttribute("selected")) {
          el.removeAttribute("selected");
          this.group.splice(this.group.indexOf(el) - 1, 1);
          console.log(this.group);
        } else {
          el.setAttribute("selected", "");
          this.group.push(el.value);
          console.log(this.group);
        }
      }
    },
  },
  computed: {
    dateObject() {
      return this.birthDate ? new Date(this.birthDate) : null;
    },
  },
};
</script>

<style lang="scss" scoped>
.error {
  color: red;
}
.valid {
  color: green;
}

::-webkit-calendar-picker-indicator {
  display: none;
}
::-webkit-inner-spin-button {
  display: none;
}
</style>>
