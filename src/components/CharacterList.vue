<template>
  <div class="select-character">
    <div
      @click="toggleDropdown"
      :class="[
        'select-character__data-input',
        {
          'select-character__data-input--no-bottom-border': isDropdownVisible,
        },
      ]"
    >
      <div class="select-character__default">
        {{ chosenCharacter }}
      </div>
      <div
        :class="[
          'select-character__arrow',
          { 'select-character__arrow--up': isDropdownVisible },
        ]"
      >
        <img src="../assets/arrow.svg" alt="arrow" />
      </div>
    </div>
    <transition name="character-list">
      <div v-if="isDropdownVisible" class="select-character__datalist">
        <ul class="select-character__list">
          <li
            @click="setName(character.name)"
            v-for="character in characters"
            :key="character.id"
            class="select-character__list-element"
          >
            {{ character.name }}
          </li>
        </ul>
      </div>
    </transition>
  </div>
</template>
<script>
import { ref, inject, toRef } from "vue";
export default {
  emits: ["chosen-name"],
  setup(props, { emit }) {
    const isDropdownVisible = ref(false);
    const chosenCharacter = ref("Pick a character");
    const characters = inject("characters");
    const charValidate = toRef(props, "charValidate");
    const toggleDropdown = () => {
      isDropdownVisible.value = !isDropdownVisible.value;
    };
    const setName = (name) => {
      chosenCharacter.value = name;
      toggleDropdown();
      emit("chosen-name", chosenCharacter.value);
    };
    return {
      toggleDropdown,
      isDropdownVisible,
      characters,
      setName,
      chosenCharacter,
      charValidate,
    };
  },
};
</script>
<style lang="scss" scoped>
$light-gray: #e0e0e0;
$hover-gray: #f4f4f4;
$red: #de212b;
.select-character {
  cursor: pointer;
  &__data-input {
    display: flex;
    border-radius: 8px;
    border: 1px solid $light-gray;
    padding: 10.5px;
    &--no-bottom-border {
      border-bottom-left-radius: 0px;
      border-bottom-right-radius: 0px;
    }
    &--invalid {
      border-color: $red;
    }
  }
  &__default {
    flex-grow: 1;
  }
  &__arrow {
    transform: rotate(180deg);
    transition: all 0.2s;
    &--up {
      transform: rotate(0deg);
      transition: all 0.2s;
    }
  }
  &__datalist {
    width: 100%;
    border: 1px solid $light-gray;
    border-top: 0px;
    border-bottom-left-radius: 8px;
    border-bottom-right-radius: 8px;
  }
  &__list {
    max-height: 22vh;
    overflow: scroll;
    list-style: none;
    @media (max-width: 1280px) {
      max-height: 16vh;
    }
  }
  &__list-element {
    width: 100%;
    line-height: 38px;
    padding: 0 10.5px;
    &:hover {
      background-color: $hover-gray;
    }
  }
}
.character-list {
  &-enter-from,
  &-leave-to {
    opacity: 0;
    max-height: 0px;
  }
  &-enter-to,
  &-leave-from {
    opacity: 1;
    max-height: 1000px;
  }
  &-enter-active,
  &-leave-active {
    transition: 0.2s;
  }
}
</style>
