<template>
  <the-navigation></the-navigation>
  <router-view @messages-list="setMessages"></router-view>
</template>

<script>
import TheNavigation from "./components/TheNavigation.vue";
import { ref, provide } from "vue";
export default {
  components: {
    TheNavigation,
  },
  setup() {
    const characters = ref(null);
    const messages = ref([]);
    fetch("https://rickandmortyapi.com/api/character")
      .then((response) => response.json())
      .then((data) => (characters.value = data.results));
    provide("characters", characters);
    const setMessages = (tab) => {
      messages.value = tab.value;
    };
    provide("messages", messages);
    return { characters, setMessages };
  },
};
</script>
<style lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Source+Sans+Pro:wght@400;600;700&display=swap");
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
#app {
  font-family: "Source Sans Pro", Helvetica, Arial, sans-serif;
}
</style>
