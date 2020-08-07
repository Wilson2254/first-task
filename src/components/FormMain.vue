<template>
  <div class="main_form">
    <!-- Модальное окно с созданным клиентом -->
    <div class="modal">
      <Modal v-if="modal" @closeModal="closeInfoModal">
        <p>Фамилия: {{this.secondName}}</p>
        <p>Имя: {{this.firstName}}</p>
        <p>Отчество: {{this.thirdName}}</p>
        <p>Дата рождения: {{this.birthDate}}</p>
        <p>Пол: {{this.gender}}</p>
        <p>Группа клиентов: {{this.group.join(' | ')}}</p>
        <p>Лечащий врач: {{this.doctor}}</p>
        <p>Отправлять смс: {{this.sms? "НЕТ" : "ДА"}}</p>
        <p>Индекс: {{this.index}}</p>
        <p>Страна: {{this.country}}</p>
        <p>Область: {{this.region}}</p>
        <p>Город: {{this.city}}</p>
        <p>Улица: {{this.street}}</p>
        <p>Дом: {{this.house}}</p>
        <p>Документ: {{this.docum}}</p>
        <p>Серия: {{this.series}}</p>
        <p>Номер: {{this.num}}</p>
        <p>Кем выдан: {{this.whoGive}}</p>
        <p>Дата выдачи: {{this.giveDate}}</p>
      </Modal>
    </div>

    <!-- Блок переключения между частями формы -->
    <div class="control">
      <div class="step">
        <a class="step_rule" @click="changeStep">Основное</a>
      </div>
      <div class="step">
        <a class="step_rule" @click="changeStep">Адрес</a>
      </div>
      <div class="step">
        <a class="step_rule" @click="changeStep">Паспорт</a>
      </div>
      <div class="step">
        <a class="step_rule" @click="submitForm">Создать</a>
      </div>
    </div>

    <!-- Форма основной информации -->
    <div class="basic" :class="{hide: this.steps != 1}">
      <h1>Основное</h1>

      <!-- Валидация фамилии -->
      <!-- Применяю класс ошибки, если инпут пустой или был стерт -->
      <div class="group" :class="{error:!$v.secondName.required && $v.secondName.$dirty}">
        <input
          v-model.trim="$v.secondName.$model"
          :class="{error:!$v.secondName.required && $v.secondName.$dirty}"
        />
        <div :class="{error:!$v.secondName.required && $v.secondName.$dirty}">
          <span>*</span>Фамилия
        </div>
      </div>

      <!-- Валидация имени  -->
      <!-- Применяю класс ошибки, если инпут пустой или был стерт -->
      <div class="group">
        <input
          v-model.trim="$v.firstName.$model"
          :class="{error:!$v.firstName.required && $v.firstName.$dirty}"
        />
        <div :class="{error:!$v.firstName.required && $v.firstName.$dirty}">
          <span>*</span>Имя
        </div>
      </div>

      <!-- Отчество -->
      <div class="group">
        <input v-model.trim="thirdName" />
        <div>Отчество</div>
      </div>

      <!-- Валидации даты рождения-->
      <!-- Применяю класс ошибки, если дата пустая или была стерта, а также на каждое изменение вызываю проверку по маске -->
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

      <!-- Валидация телефона (с помощью регулярок) -->
      <!-- Применяю класс ошибки, если телефон инпут телефона меньше 14 символов -->
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

      <!-- Пол -->
      <div class="group">
        <input v-model.trim="gender" />
        <div>Пол</div>
      </div>

      <!-- Валидация группы клиентов -->
      <!-- Применяю класс ошибки, если массив группы пустой или из него были удалены элементы -->
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
          <!-- Вывод выбранной группы -->
        </div>
        <div v-for="item in group" :key="item">- {{item}}</div>
      </div>

      <!-- Лечащий врач -->
      <div class="group">
        <select v-model="doctor">
          <option>Иванов</option>
          <option>Захаров</option>
          <option>Чернышева</option>
        </select>
        <div>Лечащий врач</div>
      </div>

      <!-- Смс -->
      <div class="group">
        <input type="checkbox" v-model="sms" />
        Не отправлять СМС
      </div>
    </div>

    <!-- Форма адреса -->
    <div class="adress" :class="{hide: this.steps != 2}">
      <h1>Адрес</h1>

      <!-- Индекс -->
      <div class="group">
        <input v-model.trim="index" />
        <div>Индекс</div>
      </div>

      <!-- Страна -->
      <div class="group">
        <input v-model.trim="country" />
        <div>Страна</div>
      </div>

      <!-- Область -->
      <div class="group">
        <input v-model.trim="region" />
        <div>Область</div>
      </div>

      <!-- Валидация города -->
      <!-- Применяю класс ошибки, если инпут пустой или был стерт -->
      <div class="group">
        <input v-model.trim="$v.city.$model" :class="{error:!$v.city.required && $v.city.$dirty}" />
        <div :class="{error:!$v.city.required && $v.city.$dirty}">
          <span>*</span>Город
        </div>
      </div>

      <!-- Улица -->
      <div class="group">
        <input v-model.trim="street" />
        <div>Улица</div>
      </div>

      <!-- Дом -->
      <div class="group">
        <input v-model.trim="house" />
        <div>Дом</div>
      </div>
    </div>

    <!-- Форма документных данных -->
    <div class="passport" :class="{hide: this.steps != 3}">
      <h1>Паспорт</h1>

      <!-- Тип документа -->
      <div div class="group">
        <select v-model="docum">
          <option>Паспорт</option>
          <option>Свидетельство о рождении</option>
          <option>Вод. удостоверение</option>
        </select>
        <div>
          <span>*</span>Тип документа
        </div>
      </div>

      <!-- Серия -->
      <div class="group">
        <input v-model.trim="series" />
        <div>Серия</div>
      </div>

      <!-- Номер -->
      <div class="group">
        <input v-model.trim="num" />
        <div>Номер</div>
      </div>

      <!-- Кем выдан -->
      <div class="group">
        <input v-model.trim="whoGive" />
        <div>Кем выдан</div>
      </div>

      <!-- Валидация даты выдачи -->
      <!-- Применяю класс ошибки, также как и с датой рождения -->
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
</template>

<script>
import Modal from "./Modal/Modal.vue"; //Подключаю модалку
import { required, maxValue, minValue } from "vuelidate/lib/validators"; //Используемые параметры у Vuelidate
// Создаю дефолтную текущую дату (чтоб вывводить ее в начале), формат ГГ-ММ-ДД
let ourDate =
  new Date().getFullYear() +
  "-" +
  String(new Date().getMonth() + 1).padStart(2, "0") +
  "-" +
  String(new Date().getDate() - 1).padStart(2, "0");
export default {
  data() {
    return {
      steps: 1, //Шаги для переключения между частями формы
      modal: false, //Для вызова модалки
      secondName: "",
      firstName: "",
      thirdName: "", //Отчество
      birthDate: ourDate,
      phone: "7",
      gender: "",
      group: [],
      doctor: "Иванов",
      sms: false,
      giveDate: ourDate,
      index: "",
      country: "",
      region: "",
      city: "",
      street: "",
      house: "",
      docum: "Паспорт",
      series: "",
      num: "",
      whoGive: "",
    };
  },
  components: {
    Modal, //Зарегестрировал Модалку
  },
  validations: {
    // Указываю параметры валидации для контректного поля, (required - не должна быть пустой)
    secondName: { required },
    firstName: { required },
    birthDateMask: {
      // Дата должна быть заполнена, не должна быть больше текущего и меньше 1900 года
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
    // Проверка на валидность всех полей, которые я указал
    submitForm() {
      if (this.$v.$invalid) {
        this.$v.$touch(); // Если есть что-то не валидное, то показываю какие
        this.steps = 1; // Переключаюсь на первую часть
        return;
      } else {
        //Если все ок, то показываю модалку
        this.showModal();
        this.steps = 2; //Просто переключаюсь на вторую часть (можно и не переключаться)
      }
    },

    // Показ модалки
    showModal() {
      this.modal = true;
    },

    // Закрытие модалки
    closeInfoModal() {
      this.modal = false;
    },

    // Использую регулярки для маски телефона
    acceptNumber() {
      let x = this.phone
        .replace(/\D/g, "") //Удаляю все кроме цифр
        .match(/(\d{0,1})(\d{0,3})(\d{0,3})(\d{0,4})/); //Маска
      if (x[1] !== 7) x[1] = 7; //Первый символ всегда будет 7
      //Так как получается массив, то я вывожу его правильно последовательностью
      this.phone = !x[2]
        ? x[1]
        : x[1] +
          "-" +
          x[2] +
          (x[3] ? "-" + x[3] : "") +
          (x[4] ? "-" + x[4] : "");
    },

    // Выбор группы клиентов
    chooseClient(e) {
      var el = e.target; //Ловлю текущую группу на которую нажал
      //Проверяю что я нажал на пунт меню
      if (
        el.tagName.toLowerCase() == "option" &&
        el.parentNode.hasAttribute("multiple")
      ) {
        e.preventDefault(); //Отключаю выбор меню с зажатым CTRL или SHIFT
        // Если атрибут был выбран, то удаляю его из массивы групп
        if (el.hasAttribute("selected")) {
          el.removeAttribute("selected");
          this.group.splice(this.group.indexOf(el.value), 1);
        } else {
          // Или наоборот добавляю выбранную группу
          el.setAttribute("selected", "");
          this.group.push(el.value);
        }
      }
    },

    // Перевелючение по частям формы
    changeStep(e) {
      var el = e.target;
      // Просто смотрю что выбрал и меняю шаг
      if (el.text == "Основное") this.steps = 1;
      if (el.text == "Адрес") this.steps = 2;
      if (el.text == "Паспорт") this.steps = 3;
    },
  },

  computed: {
    // Через эти свойства возвращаем либо правильную дату, либо ничего
    birthDateMask() {
      return this.birthDate ? new Date(this.birthDate) : null;
    },

    // Через эти свойства возвращаем либо правильную дату, либо ничего
    giveDateMask() {
      return this.giveDate ? new Date(this.giveDate) : null;
    },
  },

  // Здесь просто работа с LocalStorage (Каждый раз записываю изменения в LocalStorage)
  watch: {
    firstName(newInput) {
      localStorage.firstName = newInput;
    },
    secondName(newInput) {
      localStorage.secondName = newInput;
    },
    thirdName(newInput) {
      localStorage.thirdName = newInput;
    },
    birthDate(newInput) {
      localStorage.birthDate = newInput;
    },
    phone(newInput) {
      localStorage.phone = newInput;
    },
    gender(newInput) {
      localStorage.gender = newInput;
    },
    doctor(newInput) {
      localStorage.doctor = newInput;
    },
    sms(newInput) {
      localStorage.sms = newInput;
    },
    index(newInput) {
      localStorage.index = newInput;
    },
    country(newInput) {
      localStorage.country = newInput;
    },
    region(newInput) {
      localStorage.region = newInput;
    },
    city(newInput) {
      localStorage.city = newInput;
    },
    street(newInput) {
      localStorage.street = newInput;
    },
    house(newInput) {
      localStorage.house = newInput;
    },
    docum(newInput) {
      localStorage.docum = newInput;
    },
    series(newInput) {
      localStorage.series = newInput;
    },
    num(newInput) {
      localStorage.num = newInput;
    },
    whoGive(newInput) {
      localStorage.whoGive = newInput;
    },
    giveDate(newInput) {
      localStorage.giveDate = newInput;
    },
  },

  // Считываю из LocalStorage, если он не пустой
  mounted() {
    if (localStorage.firstName) this.firstName = localStorage.firstName;
    if (localStorage.secondName) this.secondName = localStorage.secondName;
    if (localStorage.thirdName) this.thirdName = localStorage.thirdName;
    if (localStorage.birthDate) this.birthDate = localStorage.birthDate;
    if (localStorage.phone) this.phone = localStorage.phone;
    if (localStorage.gender) this.gender = localStorage.gender;
    if (localStorage.doctor) this.doctor = localStorage.doctor;
    if (localStorage.sms) this.sms = localStorage.sms;
    if (localStorage.index) this.index = localStorage.index;
    if (localStorage.country) this.country = localStorage.country;
    if (localStorage.region) this.region = localStorage.region;
    if (localStorage.city) this.city = localStorage.city;
    if (localStorage.street) this.street = localStorage.street;
    if (localStorage.house) this.house = localStorage.house;
    if (localStorage.docum) this.docum = localStorage.docum;
    if (localStorage.series) this.series = localStorage.series;
    if (localStorage.num) this.num = localStorage.num;
    if (localStorage.whoGive) this.whoGive = localStorage.whoGive;
    if (localStorage.giveDate) this.giveDate = localStorage.giveDate;
  },
};
</script>

<style lang="scss" scoped>
// Основная часть
.main_form {
  margin: 20px;
  font-size: 14pt;
  text-transform: uppercase;
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  .basic,
  .adress,
  .passport {
    h1 {
      margin-left: 15px;
      font-size: 18pt;
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
      width: 300px;
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
        width: 300px;
        border: none;
        border-bottom: 1px solid;
        &:focus {
          outline: none;
        }
      }
      input[type="checkbox"] {
        width: 20px;
        height: 20px;
      }
    }
  }
  .control {
    margin-left: 20px;
    .step {
      margin-bottom: 50px;
      margin-right: 100px;
      border-bottom: 2px solid black;
      .step_rule {
        cursor: pointer;
        text-decoration: none;
        transition-duration: 0.2s;
        transition-delay: 0.1s;
        user-select: none;
        &:hover {
          color: gray;
        }
      }
    }
  }
  .modal {
    width: 100%;
    height: 100%;
  }
}

// Класс для скрытия часте формы
.hide {
  display: none;
  transition: opacity 0.5s ease-in-out;
}

// Подсветка ошибок
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


// Отключаю значок календаря
::-webkit-calendar-picker-indicator {
  display: none;
}

// Адаптивность для разных устройств
@media screen and (max-width: 600px) {
  .main_form {
    font-size: 12pt;
    margin: 15px 5px;
    .basic,
    .adress,
    .passport {
      h1,
      .group {
        margin-left: 5px;
      }
      .group {
        margin-top: 5px;
      }
    }

    .control {
      display: flex;
      flex-direction: row;
      flex-wrap: wrap;
      justify-content: space-between;
      margin-left: 0;
      .step {
        margin-right: 10px;
        margin-bottom: 20px;
      }
    }
  }
}
</style>>
