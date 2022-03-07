<template>
  <nav class="nav">
    <div class="nav__logo">
      <img
        class="nav__image"
        src="../assets/rickmortylogo.svg"
        alt="Rick n Morty"
      />
    </div>
    <div class="nav__links">
      <div class="nav__link-new-mess">
        <router-link to="/">New message</router-link>
      </div>
      <div class="nav__link-history">
        <router-link to="/history">History</router-link>
      </div>
    </div>
  </nav>
</template>

<script>
export default {
  setup() {},
};
</script>

<style lang="scss" scoped>
$navy: #324b72;
$green: #4eadc5;
.nav {
  display: flex;
  justify-content: space-around;
  background-color: #f5f8fb;
  color: $navy;
  font-size: 16px;
  padding: 16.25px 0;
  @media (max-width: 768px) {
    justify-content: space-between;
    padding: 14px;
  }
  &__image {
    @media (max-width: 768px) {
      width: 84px;
    }
  }
  &__links {
    display: flex;
    align-items: center;
  }
  &__link-new-mess {
    margin-right: 20px;
  }
  a {
    text-decoration: none;
    color: $navy;
    &:hover,
    &.router-link-active {
      color: $green;
    }
  }
}
</style>
