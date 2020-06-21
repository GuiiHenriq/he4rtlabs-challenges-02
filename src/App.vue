<template>
  <div id="app">
    <Header />
    <Main />
    <Footer />
  </div>
</template>

<script>
import Header from './components/Header.vue'
import Main from './components/Main.vue'
import Footer from './components/Footer.vue'

export default {
  name: 'App',
  components: {
    Header,
    Main,
    Footer
  }
}
</script>

<style lang="scss" >
@import url('https://fonts.googleapis.com/css2?family=Source+Sans+Pro:wght@300;400;600&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Noto+Sans+JP:wght@300;400;700&display=swap');

#app {
  max-width: 1280px;
  margin: 0 auto;
}

body {
  background-color: #F6F7FA !important;
}

body, input, label, button {
  font-family: 'Source Sans Pro', sans-serif;
  font-family: 'Noto Sans JP', sans-serif;
}

.btn.btn-primary {
  background-color: #6564db !important;
}

ul, li {
  list-style: none;
}

//#0d1317
//#101d42
//#232ed1
//#6564db
//#89d2dc
</style>
