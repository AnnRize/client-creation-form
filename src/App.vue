<script setup>
import { required, helpers, minLength } from "@vuelidate/validators";
import useVuelidate from "@vuelidate/core";
import { reactive, ref } from "vue";
import MyRadioButton from "./components/MyRadioButton.vue";
import MyCheckbox from "./components/MyCheckbox.vue";
import MyInput from "./components/MyInput.vue";

const isSuccess = ref(false);
const initialState = {
  firstName: "",
  lastName: "",
  fatherName: "",
  dateOfBirth: "",
  telephone: "7",
  sex: "None",
  clientGroup: [],
  doctor: "Иванов",
  sms: false,
  index: "",
  country: "",
  region: "",
  city: "",
  street: "",
  house: "",
  docType: "Паспорт",
  number: "",
  series: "",
  issuedBy: "",
  dateOfIssue: "",
};

let formData = reactive({ ...initialState });

const customRequired = helpers.withMessage(
  "Обязательно к заполнению",
  required
);

const customMinLength = helpers.withMessage(
  "Должно быть 11 символов",
  minLength(11)
);

const rules = {
  lastName: { customRequired },
  firstName: {
    customRequired,
  },
  dateOfBirth: { customRequired },
  telephone: { customRequired, customMinLength },
  clientGroup: { customRequired },
  city: { customRequired },
  docType: { customRequired },
  dateOfIssue: { customRequired },
};

const v$ = useVuelidate(rules, formData);

async function submit() {
  const isValidate = await v$.value.$validate();

  if (isValidate) {
    isSuccess.value = true;
    setTimeout(() => {
      isSuccess.value = false;
    }, 2000);

    v$.value.$reset();
    Object.assign(formData, initialState);
  }
}
</script>

<template>
  <div
    v-if="isSuccess"
    :class="$style.modal"
    @click="
      () => {
        isSuccess = false;
      }
    "
  >
    <div :class="$style.content">
      <p>Клиент успешно создан!</p>
    </div>
  </div>
  <div :class="$style.container">
    <form @submit.prevent="submit" :class="$style.form" action="">
      <h1>Форма создания клиента</h1>
      <div :class="$style.dataRow">
        <span :class="$style.name">Фамилия*</span>
        <div :class="$style.dataValidate">
          <span :class="$style.error" v-if="v$.lastName.$error">
            {{ v$.lastName.$errors[0].$message }}
          </span>
          <MyInput
            @blur="v$.lastName.$touch()"
            type="text"
            placeholder="Фамилия"
            v-model="formData.lastName"
          />
        </div>
      </div>

      <div :class="$style.dataRow">
        <span :class="$style.name">Имя*</span>
        <div :class="$style.dataValidate">
          <span :class="$style.error" v-if="v$.firstName.$error">
            {{ v$.firstName.$errors[0].$message }}
          </span>
          <MyInput
            @blur="v$.firstName.$touch()"
            type="text"
            placeholder="Имя"
            v-model="formData.firstName"
          />
        </div>
      </div>

      <div :class="$style.dataRow">
        <span :class="$style.name">Отчество</span>
        <MyInput
          type="text"
          placeholder="Отчество"
          v-model="formData.fatherName"
        />
      </div>
      <div :class="$style.dataRow">
        <span :class="$style.name">Дата рождения*</span>

        <div :class="$style.dataValidate">
          <span :class="$style.error" v-if="v$.dateOfBirth.$error">
            {{ v$.dateOfBirth.$errors[0].$message }}
          </span>
          <MyInput
            @blur="v$.dateOfBirth.$touch()"
            type="date"
            placeholder="Дата рождения"
            v-model="formData.dateOfBirth"
          />
        </div>
      </div>
      <div :class="$style.dataRow">
        <span :class="$style.name">Номер телефона*</span>
        <div :class="$style.dataValidate">
          <span :class="$style.error" v-if="v$.telephone.$error">
            {{ v$.telephone.$errors[0].$message }}
          </span>
          <MyInput
            @blur="v$.telephone.$touch()"
            type="tel"
            placeholder="Номер телефона"
            maxLength="11"
            :value="formData.telephone"
            @keypress="
              (e) => {
                if (!e.code.includes(`Digit`)) {
                  e.preventDefault();
                }
              }
            "
            @input="
              (e) => {
                if (e.target.value.length <= 1) {
                  e.target.value = `7`;
                }
                formData.telephone = e.target.value;
              }
            "
          />
        </div>
      </div>
      <div :class="$style.dataRow">
        <span :class="$style.name">Пол</span>
        <div :class="$style.radioContainer">
          <div :class="$style.radioWrapper">
            <label for="sexM">None</label>
            <MyRadioButton
              name="sex"
              id="sexNone"
              value="None"
              v-model="formData.sex"
            />
          </div>

          <div :class="$style.radioWrapper">
            <label for="sexM">М</label>
            <MyRadioButton
              name="sex"
              id="sexM"
              value="М"
              v-model="formData.sex"
            />
          </div>

          <div :class="$style.radioWrapper">
            <label for="sexF">Ж</label>
            <MyRadioButton
              name="sex"
              id="sexF"
              value="Ж"
              v-model="formData.sex"
            />
          </div>
        </div>
      </div>

      <div :class="$style.dataRow" style="align-items: flex-start">
        <span :class="$style.name">Группа клиентов*</span>
        <div :class="$style.dataValidate">
          <span :class="$style.error" v-if="v$.clientGroup.$error">
            {{ v$.clientGroup.$errors[0].$message }}
          </span>
          <select
            @blur="v$.clientGroup.$touch()"
            name=""
            id=""
            multiple
            v-model="formData.clientGroup"
          >
            <option value="VIP">VIP</option>
            <option value="Проблемные">Проблемные</option>
            <option value="ОМС">ОМС</option>
          </select>
        </div>
      </div>

      <div :class="$style.dataRow">
        <span :class="$style.name">Лечащий врач</span>
        <select name="" id="" value="Иванов" v-model="formData.doctor">
          <option value="Иванов">Иванов</option>
          <option value="Захаров">Захаров</option>
          <option value="Чернышева">Чернышева</option>
        </select>
      </div>

      <div :class="$style.dataRow">
        <label for="sms">
          <span :class="$style.name">Не отправлять СМС</span>
        </label>
        <MyCheckbox id="sms" v-model="formData.sms" />
      </div>

      <hr />
      <section :class="$style.address">
        <h2>Адрес</h2>

        <div :class="$style.dataRow">
          <span :class="$style.name">Индекс</span>
          <MyInput type="text" placeholder="Индекс" v-model="formData.index" />
        </div>
        <div :class="$style.dataRow">
          <span :class="$style.name">Страна</span>
          <MyInput
            type="text"
            placeholder="Страна"
            v-model="formData.country"
          />
        </div>
        <div :class="$style.dataRow">
          <span :class="$style.name">Область</span>
          <MyInput
            type="text"
            placeholder="Область"
            v-model="formData.region"
          />
        </div>
        <div :class="$style.dataRow">
          <span :class="$style.name">Город*</span>
          <div :class="$style.dataValidate">
            <span :class="$style.error" v-if="v$.city.$error">
              {{ v$.city.$errors[0].$message }}
            </span>
            <MyInput
              @blur="v$.city.$touch()"
              type="text"
              placeholder="Город"
              v-model="formData.city"
            />
          </div>
        </div>
        <div :class="$style.dataRow">
          <span :class="$style.name">Улица</span>
          <MyInput type="text" placeholder="Улица" v-model="formData.street" />
        </div>
        <div :class="$style.dataRow">
          <span :class="$style.name">Дом</span>
          <MyInput type="text" placeholder="Дом" v-model="formData.house" />
        </div>
      </section>

      <hr />
      <section :class="$style.passport">
        <h2>Паспорт</h2>

        <div :class="$style.dataRow">
          <span :class="$style.name">Тип документа*</span>

          <div :class="$style.dataValidate">
            <span :class="$style.error" v-if="v$.docType.$error">
              {{ v$.docType.$errors[0].$message }}
            </span>
            <select
              @blur="v$.docType.$touch()"
              class="select"
              name=""
              id=""
              v-model="formData.docType"
            >
              <option value="Паспорт">Паспорт</option>
              <option value="Свидетельство о рождении">
                Свидетельство о рождении
              </option>
              <option value="Вод. удостоверение">Вод. удостоверение</option>
            </select>
          </div>
        </div>

        <div :class="$style.dataRow">
          <span :class="$style.name">Серия</span>
          <MyInput type="text" placeholder="Серия" v-model="formData.series" />
        </div>
        <div :class="$style.dataRow">
          <span :class="$style.name">Номер</span>
          <MyInput type="text" placeholder="Номер" v-model="formData.number" />
        </div>
        <div :class="$style.dataRow">
          <span :class="$style.name">Кем выдан</span>
          <MyInput
            type="text"
            placeholder="Кем выдан"
            v-model="formData.issuedBy"
          />
        </div>
        <div :class="$style.dataRow">
          <span :class="$style.name"> Дата выдачи*</span>
          <div :class="$style.dataValidate">
            <span :class="$style.error" v-if="v$.dateOfIssue.$error">
              {{ v$.dateOfIssue.$errors[0].$message }}
            </span>
            <MyInput
              @blur="v$.dateOfIssue.$touch()"
              type="date"
              placeholder="Дата выдачи"
              v-model="formData.dateOfIssue"
            />
          </div>
        </div>
      </section>
      <hr />
      <button :class="$style.submitButton" type="submit">Создать</button>
    </form>
  </div>
</template>

<style module lang="scss">
h1 {
  font-size: clamp(1.4rem, 5vw, 2.4rem);
}

h1,
h2 {
  text-align: center;
}

.modal {
  position: fixed;
  z-index: 999;
  inset: 0;
  background-color: rgba(0, 0, 0, 0.604);
  display: flex;
  justify-content: center;
  align-items: center;
  animation: fade 0.3s ease-in 0s 1 normal both;

  @keyframes fade {
    from {
      opacity: 0;
    }
    to {
      opacity: 1;
    }
  }

  .content {
    display: flex;
    justify-content: center;
    align-items: center;
    text-align: center;
    font-size: 1.5rem;
    font-weight: 600;

    border-radius: 20px;
    background-color: var(--secondary);
    width: 200px;
    min-height: 100px;
    color: var(--primary);
  }
}

.container {
  background-color: var(--secondary);
  color: var(--primary);
  border-radius: 20px;
  padding: 10px;
  max-width: 500px;
  width: fit-content;
  margin-inline: auto;

  .form {
    display: flex;
    flex-direction: column;
    gap: 10px;
    max-width: fit-content;
    margin-inline: auto;
    font-size: 1.1rem;

    .dataRow {
      display: flex;
      align-items: flex-end;
      flex-wrap: wrap;
      gap: 10px;

      > :nth-child(2) {
        flex: 1;
      }

      .name {
        display: block;
        min-width: 19ch;
        font-weight: 600;
      }

      .radioContainer {
        min-width: 200px;
        display: flex;
        gap: 10px;

        .radioWrapper {
          min-width: fit-content;
          display: flex;
          gap: 5px;
        }
      }

      .dataValidate {
        min-width: 200px;
        display: flex;
        flex-direction: column;

        .error {
          max-width: 250px;
          font-weight: 600;
          color: red;
        }

        .inputError {
          border: 2px solid red;
        }
      }
    }

    %temp {
      display: flex;
      flex-direction: column;
      gap: 10px;
    }

    .address {
      @extend %temp;
    }
    .passport {
      @extend %temp;
    }

    .submitButton {
      margin-inline: auto;
    }
  }
}

select {
  min-width: 200px;
  background-color: var(--primary);
  color: var(--secondary);
  font-size: 1.1rem;
  overflow: auto;
  outline: none;
  border: none;
  border-radius: 5px;
  padding: 5px;

  &[multiple] {
    padding: 10px;
    max-height: 105px;
  }

  option {
    border: 2px solid var(--secondary);
    border-radius: 5px;
    text-align: center;
    padding: 0 5px;
    transition: 0.2s ease-in-out;
    white-space: normal;
  }

  option:not(:last-child) {
    margin-bottom: 5px;
  }

  option:checked,
  option:hover {
    background-color: var(--secondary);
    color: var(--primary);
  }
}
</style>
