<template>
  <div class="new-message">
    <div class="new-message__header">
      <h1 class="new-message__head-text">Send a new message</h1>
    </div>
    <div class="new-message__content">
      <form @submit.prevent="sendMessage" class="new-message__form" action="">
        <div class="new-message__input-group">
          <div class="new-message__label-group">
            <label
              :class="[
                'new-message__label',
                { 'new-message__label--invalid': !titleValidate },
              ]"
              for="title"
              >Title</label
            >
          </div>
          <input
            v-model="enteredTitle"
            :class="[
              'new-message__input',
              { 'new-message__input--invalid': !titleValidate },
            ]"
            type="text"
            placeholder="Enter title here..."
            @blur="validateTitle"
          />
          <p v-if="!titleValidate" class="new-message__invalid">
            Please enter the title
          </p>
        </div>
        <div class="new-message__input-group">
          <div class="new-message__label-group">
            <label
              :class="[
                'new-message__label',
                { 'new-message__label--invalid': !msgValidate },
              ]"
              for="message"
              >Message</label
            >
          </div>
          <textarea
            v-model="enteredMsg"
            :class="[
              'new-message__input',
              'new-message__input--textarea',
              { 'new-message__input--invalid': !msgValidate },
            ]"
            name="message"
            cols="30"
            rows="8"
            placeholder="Enter message here..."
            @blur="validateMsg"
          ></textarea>
          <p v-if="!msgValidate" class="new-message__invalid">
            Please enter the message
          </p>
        </div>
        <div class="new-message__input-group">
          <div class="new-message__label-group">
            <label
              :class="[
                'new-message__label',
                { 'new-message__label--invalid': !characterValidate },
              ]"
              for="character"
              >Character</label
            >
          </div>
          <div class="new-message__datalist">
            <character-list @chosen-name="setName"></character-list>
          </div>
        </div>
        <div class="new-message__button">
          <button class="new-message__send-button">Send</button>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
import CharacterList from "../components/CharacterList.vue";
import { ref, inject } from "vue";
export default {
  components: {
    CharacterList,
  },

  emits: ["messages-list"],
  setup(props, { emit }) {
    const characterName = ref(null);
    const enteredTitle = ref(null);
    const enteredMsg = ref(null);
    const messages = ref([]);
    const characters = inject("characters");
    if (sessionStorage.getItem("messages")) {
      messages.value = JSON.parse(sessionStorage.getItem("messages"));
    }
    const today = new Date();
    const day = String(today.getDate()).padStart(2, "0");
    const month = String(today.getMonth() + 1).padStart(2, "0");
    const year = today.getFullYear();
    const titleValidate = ref(true);
    const msgValidate = ref(true);
    const characterValidate = ref(true);
    const setName = (name) => {
      characterName.value = name;
      characterValidate.value = true;
    };
    const validateMsg = () => {
      if (enteredMsg.value === null) {
        msgValidate.value = false;
      } else if (enteredMsg.value.trim().length < 256) {
        msgValidate.value = true;
      }
    };
    const validateTitle = () => {
      const specialChars = /[`!@#$%^&*()_+\-=[\]{};':"\\|,.<>/?~]/;
      const areSpecial = specialChars.test(enteredTitle.value);
      if (!enteredTitle.value || areSpecial) {
        titleValidate.value = false;
      } else if (
        enteredTitle.value.trim().length >= 3 &&
        enteredTitle.value.trim().length <= 32
      ) {
        titleValidate.value = true;
      } else {
        titleValidate.value = false;
      }
    };
    const sendMessage = () => {
      if (!enteredMsg.value || !msgValidate.value) {
        msgValidate.value = false;
      }
      if (!characterName.value || !characterValidate.value) {
        characterValidate.value = false;
      }
      if (!enteredTitle.value || !titleValidate.value) {
        titleValidate.value = false;
      }
      if (msgValidate.value && characterValidate.value && titleValidate.value) {
        const charact = characters.value.find(
          (char) => characterName.value == char.name
        );
        messages.value.unshift({
          title: enteredTitle.value,
          message: enteredMsg.value,
          toCharacter: characterName.value,
          date: `${day}.${month}.${year}`,
          image: charact.image,
        });
        sessionStorage.setItem("messages", JSON.stringify(messages.value));
        characterName.value = null;
        enteredTitle.value = null;
        enteredMsg.value = null;
        titleValidate.value = true;
        msgValidate.value = true;
        characterValidate.value = true;
      }
    };
    emit("messages-list", messages);

    return {
      sendMessage,
      setName,
      enteredTitle,
      enteredMsg,
      validateTitle,
      titleValidate,
      msgValidate,
      characterValidate,
      validateMsg,
    };
  },
};
</script>

<style lang="scss" scoped>
$navy: #324b72;
$light-gray: #e0e0e0;
$red: #de212b;
.new-message {
  color: $navy;
  width: 460px;
  margin: 0 auto;
  @media (max-width: 768px) {
    width: 360px;
  }
  @media (max-width: 320px) {
    width: 300px;
  }
  &__header {
    text-align: center;
    padding: 30px;
    @media (max-width: 1280px) {
      padding: 20px;
    }
    @media (max-width: 768px) {
      text-align: left;
      padding: 30px 0;
      width: 40vw;
    }
  }
  &__head-text {
    font-size: 36px;
    @media (max-width: 768px) {
      font-size: 24px;
    }
  }
  &__form {
    font-size: 14px;
    display: flex;
    flex-direction: column;
  }
  &__input {
    width: 100%;
    border-radius: 8px;
    border: 1px solid $light-gray;
    padding: 10.5px;
    outline: none;
    &::placeholder {
      color: $light-gray;
      font-size: 14px !important;
    }
    &--textarea {
      resize: none;
      font-family: "Source Sans Pro", Helvetica, Arial, sans-serif;
    }
    &--invalid {
      border-color: $red;
    }
  }
  &__input-group {
    margin-bottom: 16px;
  }
  &__label-group {
    margin-bottom: 10px;
  }
  &__label--invalid {
    color: $red;
  }
  &__button {
    display: flex;
    justify-content: flex-end;
    flex-grow: 1;
  }
  &__send-button {
    width: 90px;
    height: 38px;
    margin: 20px 0;
    color: white;
    border-radius: 19px;
    border: none;
    background-color: #4eadc5;
    cursor: pointer;
  }
  &__datalist {
    width: 100%;
  }
  &__invalid {
    margin-top: 10px;
    font-size: 12px;
    color: $red;
  }
}
</style>
