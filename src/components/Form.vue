<template>
  <form @submit="onSubmit" class="form" name="Form">
    <h1 class="form__header">Форма обратной связи</h1>
    <section>
      <div class="field">
        <label for="Name" class="field__label">Ваше ФИО</label>
        <input
          class="field__input"
          type="text"
          name="Name"
          id="Name"
          v-model="name"
          placeholder="ФИО"
        />
      </div>
      <div class="field">
        <label for="Message" class="field__label">Текст сообщения</label>
        <input
          class="field__input"
          type="text"
          name="Message"
          id="Message"
          v-model="message"
          placeholder="текст"
        />
      </div>
      <div class="field">
        <label for="Email" class="field__label">Ваш Email</label>
        <input
          class="field__input"
          type="text"
          name="Email"
          id="Email"
          v-model="email"
          placeholder="email"
        />
      </div>
      <div class="field">
        <label for="City" class="field__label">Город</label>
        <select
          class="field__input field__input_type_select"
          name="City"
          id="City"
        >
          <option value="Санкт-Петербург">Санкт-Петербург</option>
          <option value="Москва">Москва</option>
          <option value="Екатеринбург">Екатеринбург</option>
        </select>
      </div>
      <div class="field field_type_checkbox">
        <label
          for="SelfMessage"
          class="field__label field__label_type_checkbox"
        >
          <input
            class="field__input field__input_type_toggle"
            type="checkbox"
            name="SelfMessage"
            id="SelfMessage"
            v-model="selfMessage"
          />
          Отправить себе копию письма</label
        >
      </div>
      <div class="field field_type_checkbox">
        <label for="Accept" class="field__label field__label_type_checkbox">
          <input
            class="field__input field__input_type_checkbox"
            type="checkbox"
            id="Accept"
            name="Accept"
            v-model="accept"
          />
          <svg class="field__input-icon_type_checkbox">
            <rect x="0" y="0" width="20" height="20" />
          </svg>
          Согласие на обработку персональных данных</label
        >
      </div>
    </section>
    <section>
      <div class="field">
        <button
          @click.prevent="onSubmit"
          class="field__input button"
          type="submit"
          name="Submit"
        >
          Отправить
        </button>
      </div>
    </section>
  </form>
</template>

<script>
export default {
  data() {
    return {
      name: "",
      message: "",
      email: "",
      city: "",
      accept: true,
      selfMessage: false,
    };
  },
  methods: {
    onSubmit() {
      alert("Вы великолепны!");
    },
  },
};
</script>

<style>
.form {
  padding: 20px;
  margin: auto;
  max-width: 50%;
  background-color: rgb(162, 190, 214);
  border: none;
  font-family: Arial, sans-serif;
}
.form__legend {
  position: relative;
  top: 15px;
  font-size: 2em;
}
.field {
  margin: 15px 10px;
  display: flex;
  flex-direction: column;
  align-items: start;
}

.field__input {
  padding: 5px;
  border-radius: 0;
  border: 1px solid black;
  appearance: none;
  box-sizing: border-box;
}

.field__input:focus {
  outline: black 1px solid;
}
.field__input:active {
  outline: black 1px solid;
}
.field__label_type_checkbox {
  display: flex;
}
.field__input_type_checkbox,
.field__input_type_checkbox:focus,
.field__input_type_checkbox:active {
  border: none;
  outline: none;
}

.field__input-icon_type_checkbox {
  fill: white;
  width: 20px;
  height: 20px;
  margin-right: 5px;
  border: 1px solid black;
  box-sizing: border-box;
  transition: all 0.3s linear;
}

.field__input_type_checkbox:checked + .field__input-icon_type_checkbox {
  fill: black;
  border: 1px solid white;
}

.field__input_type_select {
  background-image: url("/arrow.svg");
  background-repeat: no-repeat;
  background-position: top 50% right 5px;
  appearance: none;
  padding-right: 30px;
  background-color: white;
}
.field__input_type_toggle {
  position: relative;
  width: 40px;
  height: 21px;
  margin-right: 5px;
  background: #ffffff;
  border-radius: 10px;
  transition: all 0.3s cubic-bezier(0.2, 0.85, 0.32, 1.2);
}

.field__input_type_toggle::after {
  content: "";
  position: absolute;
  left: 3px;
  top: 2px;
  width: 15px;
  height: 15px;
  background-color: black;
  border-radius: 50%;
  transform: translateX(0);
  transition: all 0.3s cubic-bezier(0.2, 0.85, 0.32, 1.2);
}

.field__input_type_toggle:checked::after {
  transform: translateX(calc(100% + 3px));
  background-color: #fff;
}
.field__input_type_toggle:checked {
  background-color: black;
  border: 1px solid white;
}
</style>
