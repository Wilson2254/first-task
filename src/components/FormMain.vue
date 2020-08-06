<template>
  <div class="main_form">
    <div class="basic">
      <h1>Основное</h1>

      <div class="field_row">
        <div class="group">
          <input
            v-model.trim="$v.secondName.$model"
            :class="{error:!$v.secondName.required && $v.secondName.$dirty}"
          />
          <div :class="{error:!$v.secondName.required && $v.secondName.$dirty}">
            <span>*</span>Фамилия
          </div>
        </div>

        <div class="group">
          <input
            v-model.trim="$v.firstName.$model"
            :class="{error:!$v.firstName.required && $v.firstName.$dirty}"
          />
          <div :class="{error:!$v.firstName.required && $v.firstName.$dirty}">
            <span>*</span>Имя
          </div>
        </div>

        <div class="group">
          <input v-model.trim="thirdName" />
          <div>Отчество</div>
        </div>
      </div>

      <div class="field_row">
        <div class="group">
          <input
            type="date"
            v-model="birthDate"
            @input="$v.birthDateMask.$touch"
            :class="{error:!$v.birthDateMask.required && $v.birthDateMask.$dirty || $v.birthDateMask.$error}"
          />
          <div
            :class="{error: !$v.birthDateMask.required && $v.birthDateMask.$dirty || $v.birthDateMask.$error}"
          >
            <span>*</span>Дата рождения
          </div>
        </div>

        <div class="group">
          <input
            placeholder="Номер телефона"
            v-model="phone"
            @input="acceptNumber"
            :class="{error:this.phone.length != 14 && $v.phone.$dirty}"
          />
          <div :class="{error:this.phone.length != 14 && $v.phone.$dirty}">
            <span>*</span>Номер телефона
          </div>
        </div>

        <div class="group">
          <input v-model.trim="gender" />
          <div>Пол</div>
        </div>
      </div>

      <div class="field_row">
        <div class="group">
          <select
            multiple
            @mousedown="chooseClient"
            v-model="group"
            :class="{error:this.group.length == 0 && !$v.group.required && $v.group.$dirty}"
          >
            <option value="VIP">VIP</option>
            <option value="Проблемные">Проблемные</option>
            <option value="ОМС">ОМС</option>
          </select>
          <div :class="{error:this.group.length == 0 && !$v.group.required && $v.group.$dirty}">
            <span>*</span>Группа клиентов
          </div>
          <div v-for="item in group" :key="item">- {{item}}</div>
        </div>

        <div class="group">
          <select>
            <option>Иванов</option>
            <option>Захаров</option>
            <option>Чернышева</option>
          </select>
          <div>Лечащий врач</div>
        </div>

        <div class="group">
          <input type="checkbox" />
          Не отправлять СМС
        </div>
      </div>
    </div>
    <div class="adress">
      <h1>Адрес</h1>
      <div class="field_row">
        <div class="group">
          <input v-model.trim="index" />
          <div>Индекс</div>
        </div>

        <div class="group">
          <input v-model.trim="country" />
          <div>Страна</div>
        </div>

        <div class="group">
          <input v-model.trim="region" />
          <div>Область</div>
        </div>
      </div>

      <div class="field_row">
        <div class="group">
          <input v-model.trim="$v.city.$model" :class="{error:!$v.city.required && $v.city.$dirty}" />
          <div :class="{error:!$v.city.required && $v.city.$dirty}">
            <span>*</span>Город
          </div>
        </div>

        <div class="group">
          <input v-model.trim="street" />
          <div>Улица</div>
        </div>

        <div class="group">
          <input v-model.trim="house" />
          <div>Дом</div>
        </div>
      </div>
    </div>

    <div class="passport">
      <h1>Паспорт</h1>

      <div class="field_row">
        <div div class="group">
          <select>
            <option>Паспорт</option>
            <option>Свидетельство о рождении</option>
            <option>Вод. удостоверение</option>
          </select>
          <div>
            <span>*</span>Тип документа
          </div>
        </div>

        <div class="group">
          <input v-model.trim="series" />
          <div>Серия</div>
        </div>

        <div class="group">
          <input v-model.trim="num" />
          <div>Номер</div>
        </div>
      </div>
      <div class="field_row">
        <div class="group">
          <input v-model.trim="whoGive" />
          <div>Кем выдан</div>
        </div>

        <div class="group">
          <input
            type="date"
            v-model="giveDate"
            @input="$v.giveDateMask.$touch"
            :class="{error:!$v.giveDateMask.required && $v.giveDateMask.$dirty || $v.giveDateMask.$error}"
          />
          <div
            :class="{error:!$v.giveDateMask.required && $v.giveDateMask.$dirty || $v.giveDateMask.$error}"
          >
            <span>*</span>Дата выдачи
          </div>
        </div>
      </div>
    </div>
    <div><button @click="submitForm">Отправить</button></div>
  </div>
</template>

<script>
import { required, maxValue, minValue } from "vuelidate/lib/validators";
let ourDate =
  new Date().getFullYear() +
  "-" +
  String(new Date().getMonth() + 1).padStart(2, "0") +
  "-" +
  String(new Date().getDate()).padStart(2, "0");
export default {
  data() {
    return {
      steps: 1,
      secondName: "",
      firstName: "",
      thirdName: "", //Отчество
      birthDate: ourDate,
      phone: "7",
      gender: "",
      group: [],
      giveDate: ourDate,
      index: "",
      country: "",
      region: "",
      city: "",
      street: "",
      house: "",
      series: "",
      num: "",
      whoGive: "",
    };
  },
  validations: {
    secondName: { required },
    firstName: { required },
    birthDateMask: {
      required,
      maxValue: maxValue(new Date()),
      minValue: minValue(new Date("1900-01-01")),
    },
    phone: { required },
    group: { required },
    city: { required },
    giveDateMask: {
      required,
      maxValue: maxValue(new Date()),
      minValue: minValue(new Date("1920-01-01")),
    },
  },
  methods: {
    submitForm() {
      if (this.$v.$invalid) {
        this.$v.$touch();
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
          this.group.splice(this.group.indexOf(el.value), 1);
        } else {
          el.setAttribute("selected", "");
          this.group.push(el.value);
        }
      }
    },
  },
  computed: {
    birthDateMask() {
      return this.birthDate ? new Date(this.birthDate) : null;
    },
    giveDateMask() {
      return this.giveDate ? new Date(this.giveDate) : null;
    },
  },
};
</script>

<style lang="scss" scoped>
.main_form {
  margin: 20px;
  font-size: 14pt;
  text-transform: uppercase;
  display: flex;
  flex-direction: column;
  align-items: center;

  .basic,
  .adress,
  .passport {
    h1 {
      text-align: center;
      font-size: 22pt;
    }
    .field_row {
      display: flex;
      flex-direction: row;
      flex-wrap: wrap;
    }
    select {
      overflow: hidden;
      border: 1px solid;
      text-align: center;
      width: 200px;
      font-size: 14pt;
    }
    span {
      font-weight: 600;
      color: red;
    }
    .group {
      margin-bottom: 30px;
      margin: 15px 15px;
      input:not([type="checkbox"]) {
        font-size: 14pt;
        display: block;
        width: 200px;
        border: none;
        border-bottom: 1px solid;
        &:focus {
          outline: none;
          background-color: #fffff0;
        }
      }
      input[type="checkbox"] {
        width: 20px;
        height: 20px;
      }
    }
  }
}

.error {
  color: red;
  border-bottom-color: red;
  animation: shake 0.82s cubic-bezier(0.36, 0.07, 0.19, 0.97) both;
  transform: translate3d(0, 0, 0);
  backface-visibility: hidden;
  perspective: 1000px;
  @keyframes shake {
    10%,
    90% {
      transform: translate3d(-1px, 0, 0);
    }

    20%,
    80% {
      transform: translate3d(2px, 0, 0);
    }

    30%,
    50%,
    70% {
      transform: translate3d(-4px, 0, 0);
    }

    40%,
    60% {
      transform: translate3d(4px, 0, 0);
    }
  }
}

::-webkit-calendar-picker-indicator {
  display: none;
}
::-webkit-inner-spin-button {
  display: none;
}
</style>>
