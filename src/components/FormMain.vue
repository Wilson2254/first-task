<template>
  <div class="main_form">
    <!-- Модальное окно с созданным клиентом -->
    <div class="modal">
      <Modal v-if="modal" @closeModal="closeInfoModal">
        <div
          v-for="(name, value) in info"
          :key="value"
        >{{value }} : {{ name !== "" ? name : "Не заполнено" }}</div>
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
      <div class="group" :class="{error:!$v.info.secondName.required && $v.info.secondName.$dirty}">
        <input
          v-model.trim="$v.info.secondName.$model"
          :class="{error:!$v.info.secondName.required && $v.info.secondName.$dirty}"
        />
        <div :class="{error:!$v.info.secondName.required && $v.info.secondName.$dirty}">
          <span>*</span>Фамилия
        </div>
      </div>

      <!-- Валидация имени  -->
      <!-- Применяю класс ошибки, если инпут пустой или был стерт -->
      <div class="group">
        <input
          v-model.trim="$v.info.firstName.$model"
          :class="{error:!$v.info.firstName.required && $v.info.firstName.$dirty}"
        />
        <div :class="{error:!$v.info.firstName.required && $v.info.firstName.$dirty}">
          <span>*</span>Имя
        </div>
      </div>

      <!-- Отчество -->
      <div class="group">
        <input v-model.trim="info.thirdName" />
        <div>Отчество</div>
      </div>

      <!-- Валидации даты рождения-->
      <!-- Применяю класс ошибки, если дата пустая или была стерта, а также на каждое изменение вызываю проверку по маске -->
      <div class="group">
        <input
          type="date"
          v-model="info.birthDate"
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
          v-model="info.phone"
          @input="acceptNumber"
          :class="{error:this.info.phone.length != 14 && $v.info.phone.$dirty}"
        />
        <div :class="{error:this.info.phone.length != 14 && $v.info.phone.$dirty}">
          <span>*</span>Номер телефона
        </div>
      </div>

      <!-- Пол -->
      <div class="group">
        <input v-model.trim="info.gender" />
        <div>Пол</div>
      </div>

      <!-- Валидация группы клиентов -->
      <!-- Применяю класс ошибки, если массив группы пустой или из него были удалены элементы -->
      <div class="group">
        <select
          multiple
          @mousedown="chooseClient"
          v-model="info.group"
          :class="{error:this.info.group.length == 0 && !$v.info.group.required && $v.info.group.$dirty}"
        >
          <option value="VIP">VIP</option>
          <option value="Проблемные">Проблемные</option>
          <option value="ОМС">ОМС</option>
        </select>
        <div
          :class="{error:this.info.group.length == 0 && !$v.info.group.required && $v.info.group.$dirty}"
        >
          <span>*</span>Группа клиентов
          <!-- Вывод выбранной группы -->
        </div>
        <div v-for="item in info.group" :key="item">- {{item}}</div>
      </div>

      <!-- Лечащий врач -->
      <div class="group">
        <select v-model="info.doctor">
          <option>Иванов</option>
          <option>Захаров</option>
          <option>Чернышева</option>
        </select>
        <div>Лечащий врач</div>
      </div>

      <!-- Смс -->
      <div class="group">
        <input type="checkbox" v-model="info.sms" />
        Не отправлять СМС
      </div>
    </div>

    <!-- Форма адреса -->
    <div class="adress" :class="{hide: this.steps != 2}">
      <h1>Адрес</h1>

      <!-- Индекс -->
      <div class="group">
        <input v-model.trim="info.index" />
        <div>Индекс</div>
      </div>

      <!-- Страна -->
      <div class="group">
        <input v-model.trim="info.country" />
        <div>Страна</div>
      </div>

      <!-- Область -->
      <div class="group">
        <input v-model.trim="info.region" />
        <div>Область</div>
      </div>

      <!-- Валидация города -->
      <!-- Применяю класс ошибки, если инпут пустой или был стерт -->
      <div class="group">
        <input
          v-model.trim="$v.info.city.$model"
          :class="{error:!$v.info.city.required && $v.info.city.$dirty}"
        />
        <div :class="{error:!$v.info.city.required && $v.info.city.$dirty}">
          <span>*</span>Город
        </div>
      </div>

      <!-- Улица -->
      <div class="group">
        <input v-model.trim="info.street" />
        <div>Улица</div>
      </div>

      <!-- Дом -->
      <div class="group">
        <input v-model.trim="info.house" />
        <div>Дом</div>
      </div>
    </div>

    <!-- Форма документных данных -->
    <div class="passport" :class="{hide: this.steps != 3}">
      <h1>Паспорт</h1>

      <!-- Тип документа -->
      <div div class="group">
        <select v-model="info.docum">
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
        <input v-model.trim="info.series" />
        <div>Серия</div>
      </div>

      <!-- Номер -->
      <div class="group">
        <input v-model.trim="info.num" />
        <div>Номер</div>
      </div>

      <!-- Кем выдан -->
      <div class="group">
        <input v-model.trim="info.whoGive" />
        <div>Кем выдан</div>
      </div>

      <!-- Валидация даты выдачи -->
      <!-- Применяю класс ошибки, также как и с датой рождения -->
      <div class="group">
        <input
          type="date"
          v-model="info.giveDate"
          @input="$v.giveDateMask.$touch"
          :class="{error:!$v.giveDateMask.required && $v.giveDateMask.$dirty }"
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
      info: {
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
      },
    };
  },
  components: {
    Modal, //Зарегестрировал Модалку
  },
  validations: {
    info: {
      // Указываю параметры валидации для контректного поля, (required - не должна быть пустой)
      secondName: { required },
      firstName: { required },
      phone: { required },
      group: { required },
      city: { required },
    },
    birthDateMask: {
      // Дата должна быть заполнена, не должна быть больше текущего и меньше 1900 года
      required,
      maxValue: maxValue(new Date()),
      minValue: minValue(new Date("1900-01-01")),
    },
    giveDateMask: {
      required,
      maxValue: maxValue(new Date()),
      minValue: minValue(new Date("1920-01-01")),
    },
  },
  methods: {
    // Проверка на валидность всех полей, которые я указал
    submitForm() {
      if (this.$v.info.$invalid) {
        this.$v.info.$touch(); // Если есть что-то не валидное, то показываю какие
        this.steps = 1; // Переключаюсь на первую часть
        return;
      } else {
        //Если все ок, то показываю модалку
        this.showModal();
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
      let x = this.info.phone
        .replace(/\D/g, "") //Удаляю все кроме цифр
        .match(/(\d{0,1})(\d{0,3})(\d{0,3})(\d{0,4})/); //Маска
      if (x[1] !== 7) x[1] = 7; //Первый символ всегда будет 7
      //Так как получается массив, то я вывожу его правильно последовательностью
      this.info.phone = !x[2]
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
          this.info.group.splice(this.info.group.indexOf(el.value), 1);
        } else {
          // Или наоборот добавляю выбранную группу
          el.setAttribute("selected", "");
          this.info.group.push(el.value);
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
      return this.info.birthDate ? new Date(this.info.birthDate) : null;
    },

    // Через эти свойства возвращаем либо правильную дату, либо ничего
    giveDateMask() {
      return this.info.giveDate ? new Date(this.info.giveDate) : null;
    },
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
  justify-content: center;
  align-items: center;
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
          color: white;
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
