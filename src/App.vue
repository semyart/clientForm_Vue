<template>
  <div class="container">
    <h1 class="h1">Создание клиента</h1>

    <form class="form" @submit.prevent="submitHandler">
      <div class="attribute-container">
        <h2 class="attribute-header">Личные данные</h2>
        <label class="label">
          Фамилия*
          <input
            type="text"
            class="input"
            placeholder="Иванов"
            v-model.trim="name"
            :class="{ input_invalid: v$.name.$errors.length }"
          />
          <div class="error-text" v-if="v$.name.$error">Введите фамилию</div>
        </label>
        <label class="label">
          Имя*
          <input
            type="text"
            class="input"
            placeholder="Иван"
            v-model.trim="secondName"
            :class="{ input_invalid: v$.secondName.$errors.length }"
          />
          <div class="error-text" v-if="v$.secondName.$error">Введите имя</div>
        </label>
        <label class="label">
          Отчество
          <input type="text" class="input" placeholder="Иванович" />
        </label>
        <label class="label">
          Дата рождения*
          <input
            type="date"
            class="input input-date"
            v-model.trim="birthDate"
            :class="{ input_invalid: v$.birthDate.$errors.length }"
          />
          <div class="error-text" v-if="v$.birthDate.$error">
            Введите дату рождения
          </div>
        </label>
        <label class="label">
          Номер телефона*
          <input
            type="tel"
            class="input input-tel"
            placeholder="7"
            v-model.trim="tel"
            :class="{ input_invalid: v$.tel.$errors.length }"
            @keydown="inputMask"
          />
          <div class="error-text" v-if="v$.tel.$error">
            Введите номер телефона
          </div>
        </label>
        <span class="label">
          Пол
          <div class="radio-container">
            <label class="label label-radio">
              <input type="radio" name="gender" class="input input-radio" />
              Мужской
            </label>
            <label class="label label-radio">
              <input type="radio" name="gender" class="input input-radio" />
              Женский
            </label>
          </div>
        </span>
        <label class="label">
          Группа клиентов*
          <select
            class="select multiple-select"
            multiple
            v-model.trim="clientGroup"
            :class="{ input_invalid: v$.clientGroup.$errors.length }"
          >
            <option>VIP</option>
            <option>Проблемные</option>
            <option>ОМС</option>
          </select>
          <div class="error-text" v-if="v$.clientGroup.$error">
            Выберите группу
          </div>
        </label>
        <span class="label">
          Лечащий врач
          <CustomSelect
            :options="['Иванов', 'Захаров', 'Чернышева']"
            :default="'Иванов'"
          />
        </span>
        <label class="label label-placeholder">
          Не отправлять СМС
          <input type="checkbox" class="input-placeholder" />
        </label>
      </div>

      <div class="attribute-container">
        <h2 class="attribute-header">Адрес</h2>
        <label class="label">
          Индекс
          <input type="number" class="input" placeholder="420122" />
        </label>
        <label class="label">
          Страна
          <input type="text" class="input" placeholder="Россия" />
        </label>
        <label class="label">
          Область
          <input type="text" class="input" placeholder="Республика Татарстан" />
        </label>
        <label class="label">
          Город*
          <input
            type="text"
            class="input"
            placeholder="Казань"
            v-model.trim="city"
            :class="{ input_invalid: v$.city.$errors.length }"
          />
          <div class="error-text" v-if="v$.city.$error">
            Введите название города
          </div>
        </label>
        <label class="label">
          Улица
          <input type="text" class="input" placeholder="Пушкина" />
        </label>
        <label class="label">
          Дом
          <input type="number" class="input" placeholder="12" />
        </label>
      </div>

      <div class="attribute-container">
        <h2 class="attribute-header">Документ</h2>
        <label class="label">
          Тип документа
          <CustomSelect
            :options="[
              'Паспорт',
              'Свидетельство о рождении',
              'Вод. удостоверение',
            ]"
            :default="'Паспорт'"
          />
        </label>
        <label class="label">
          Серия
          <input type="number" class="input" placeholder="9220" />
        </label>
        <label class="label">
          Номер
          <input type="number" class="input" placeholder="422412" />
        </label>
        <label class="label">
          Кем выдан
          <input
            type="text"
            class="input"
            placeholder="Отделом МВД по г.Казань"
          />
        </label>
        <label class="label">
          Дата выдачи*
          <input
            type="date"
            class="input input-date"
            v-model.trim="documentDate"
            :class="{ input_invalid: v$.documentDate.$errors.length }"
          />
          <div class="error-text" v-if="v$.documentDate.$error">
            Введите дату выдачи
          </div>
        </label>
      </div>

      <button type="submit" class="btn-submit">Создать</button>
    </form>

    <a href="https://medods.ru/" target="_blank" class="logo-link">
      <img class="logo-img" src="./assets/logo.png" alt="Логотип MEDODS" />
      <span class="logo-text">MEDODS</span>
    </a>
  </div>
</template>

<script>
import CustomSelect from "./components/customSelect.vue";
import useVuelidate from "@vuelidate/core";
import { required, maxLength, minLength } from "@vuelidate/validators";

export default {
  components: {
    CustomSelect,
  },
  data() {
    return {
      v$: useVuelidate(),
      name: "",
      secondName: "",
      birthDate: "",
      tel: "",
      clientGroup: "",
      city: "",
      documentDate: "",
    };
  },
  validations() {
    return {
      name: { required },
      secondName: { required },
      birthDate: { required },
      tel: { required, maxLength: maxLength(11), minLength: minLength(11) },
      clientGroup: { required },
      city: { required },
      documentDate: { required },
    };
  },
  methods: {
    submitHandler() {
      this.v$.$validate();
      if (!this.v$.$error) {
        alert("Клиент успешно создан!");
      }
    },

    inputMask() {
      if (this.tel === "") {
        this.tel = "7";
      }
    },
  },
};
</script>

<style>
@import "./assets/normalize.css";
@import url("https://fonts.googleapis.com/css2?family=Istok+Web:wght@700&display=swap");
@import "./assets/style.css";
</style>
