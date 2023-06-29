<script>
import AppHeader from './components/AppHeader.vue'
import MainComp from './components/MainComp.vue';
import FooterComp from './components/FooterComp.vue';

export default {
  name: "App",
  components: {

    AppHeader,
    MainComp,
    FooterComp

  }
}

</script>

<template>
  <AppHeader />
  <main>
    <MainComp />
  </main>
  <FooterComp />
</template>

<style lang="scss">
@use './style/main.scss';
</style>
