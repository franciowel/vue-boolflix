<template>
  <div id="app">
    <header>
      <HeaderPart @search="searchFilm" />
    </header>
    <main>
      <MainContent :text="userText" />
    </main>
  </div>
</template>

<script>
import HeaderPart from './components/Headerpart.vue'
import MainContent from './components/MainContent.vue'
export default {
  name: 'App',
  components: {
    HeaderPart,
    MainContent,
  },
  data() {
    return {
      userText: '',
    }
  },
  methods: {
    searchFilm(text) {
      this.userText = '';
      this.userText = text;
    }
  }
}
</script>

<style lang="scss">
@import './assets/style.scss';
</style>