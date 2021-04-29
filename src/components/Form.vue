<template>
  <form action="" class="form" name="Form">
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
        <label for="SelfMessage" class="field__label">
          <input
            class="field__input field__input_type_checkbox"
            type="checkbox"
            name="SelfMessage"
            id="SelfMessage"
            v-model="selfMessage"
          /><span class="field__input-checkbox-pic">
            <svg
              xmlns="http://www.w3.org/2000/svg"
              width="20"
              height="20"
              viewBox="0 0 20 20"
            >
              <line
                x1="14"
                y1="14"
                x2="25"
                y2="5"
                stroke="#000"
                stroke-width="2"
                stroke-linecap="round"
                stroke-miterlimit="10"
              ></line>
              <line
                x1="10"
                y1="10"
                x2="14"
                y2="14"
                stroke="#000"
                stroke-width="2"
                stroke-linecap="round"
                stroke-miterlimit="10"
              ></line></svg></span
          >Отправить себе копию письма</label
        >
      </div>
      <div class="field field_type_checkbox">
        <label for="Accept" class="field__label">
          <input
            class="field__input field__input_type_checkbox"
            type="checkbox"
            id="Accept"
            name="Accept"
            v-model="accept"
          /><span class="field__input-checkbox-pic">
            <svg
              xmlns="http://www.w3.org/2000/svg"
              width="20"
              height="20"
              viewBox="0 0 20 20"
            >
              <line
                x1="14"
                y1="14"
                x2="25"
                y2="5"
                stroke="#000"
                stroke-width="2"
                stroke-linecap="round"
                stroke-miterlimit="10"
              ></line>
              <line
                x1="10"
                y1="10"
                x2="14"
                y2="14"
                stroke="#000"
                stroke-width="2"
                stroke-linecap="round"
                stroke-miterlimit="10"
              ></line></svg></span
          >Согласие на обработку персональных данных</label
        >
      </div>
    </section>
    <section>
      <div class="field">
        <button class="field__input button" type="submit" name="Submit">
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
.field__label {
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
.field_type_checkbox {
  flex-direction: row;
}
.field__input_type_checkbox {
  border: none;
  outline: none;
}
.field__input_type_checkbox:focus {
  border: none;
  outline: none;
}
.field__input_type_checkbox:active {
  border: none;
  outline: none;
}
.field__input-checkbox-pic {
  width: 20px;
  background-color: white;
}
.field__input_type_checkbox:checked + .field__input-checkbox-pic {
  opacity: 0;
}

.field__input_type_select {
  background-image: url("/arrow.svg");
  background-repeat: no-repeat;
  background-position: top 50% right 5px;
  appearance: none;
  padding-right: 30px;
  background-color: white;
}
</style>
