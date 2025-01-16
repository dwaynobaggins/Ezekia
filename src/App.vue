<script>
import SpacePage from './components/pages/SpacePage.vue';
import IconBurger from './components/icons/IconBurger.vue';

export default {
  components: {
    SpacePage, IconBurger
  },
};

</script>

<template>
  <div class="header-wrapper">
    <header class="header-wrapper__header flex flex--justify">
      <div class="flex flex--align-items-center">
        <img alt="Vue logo" class="header-wrapper__header__logo" src="./assets/logo.svg" width="60" height="60" />
        <h2>Natural History Museum</h2>
      </div>
      <div class="flex flex--align-items-center">
        <IconBurger width="40" height="40" fill="var(--vt-c-black)"></IconBurger>
      </div>
    </header>
  </div>
  <div class="container">
    <main>
      <SpacePage />
    </main>
  </div>
</template>

<style lang="scss" scoped>

.header-wrapper {
  z-index: 10;
  position: sticky;
  top: 0;

  &__header {
    background-color: lightgray;
    line-height: 1.5;
    width: 100%;
    outline: none;
    padding: 16px;
    
    &__logo {
      display: block;
      margin-right: 32px;
    }
  }

}

</style>
