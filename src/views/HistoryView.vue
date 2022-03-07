<template>
  <div class="msg-history">
    <p v-if="messages.length === 0" class="msg-history__nomess">
      No messages yet
    </p>
    <div class="msg-history__header">
      <h1 class="msg-history__head-text">Message history</h1>
    </div>
    <base-card v-for="mess in messages" :key="mess.title">
      <div class="msg-history__message">
        <div class="msg-history__title">
          {{ mess.title }}
        </div>
        <div class="msg-history__details">
          <div class="msg-history__photo">
            <img
              class="msg-history__image"
              :src="mess.image"
              :alt="mess.toCharacter"
            />
          </div>
          <div class="msg-history__moreinfo">
            <div class="msg-history__sentto">
              Sent To: {{ mess.toCharacter }}
            </div>
            <div class="msg-history__date">Date: {{ mess.date }}</div>
          </div>
        </div>
        <div class="msg-history__msg-context">
          {{ mess.message }}
        </div>
      </div>
    </base-card>
  </div>
</template>

<script>
import { inject } from "vue";
import BaseCard from "../components/UI/BaseCard.vue";
export default {
  components: {
    BaseCard,
  },
  setup() {
    const messages = inject("messages");
    return { messages };
  },
};
</script>

<style lang="scss" scoped>
$navy: #324b72;
$light-gray: #e0e0e0;

.msg-history {
  color: $navy;
  width: 460px;
  font-size: 14px;
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
    }
  }
  &__head-text {
    font-size: 36px;
    @media (max-width: 768px) {
      font-size: 24px;
    }
  }
  &__title {
    font-weight: 600;
  }
  &__message {
    padding: 25px;
    min-height: 330px;
    margin-bottom: 40px;
  }
  &__details {
    display: flex;
    padding: 20px 0;
  }
  &__photo {
    margin-right: 15px;
  }
  &__moreinfo {
    display: flex;
    flex-direction: column;
    justify-content: center;
  }
  &__image {
    width: 48px;
    border-radius: 24px;
  }
  &__msg-context {
    font-size: 16px;
  }
  &__nomess {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
  }
}
</style>
