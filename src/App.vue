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
@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@300;400&display=swap');

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
  font-family: 'Roboto', sans-serif !important;
}

ul, li {
  list-style: none;
}

.btn.btn-primary {
  border: none !important;
  color: #9163CC !important;
  background-color: #fff !important;
  box-shadow: 0 1px 1px rgba(0, 0, 0, 0.12), 0 1px 2px rgba(0, 0, 0, 0.24);
  transition: all 0.3s cubic-bezier(.25,.8,.25,1);
  //background-color: #6564db !important;
}

.btn.btn-primary:hover {
  box-shadow: 0 3px 6px rgba(0,0,0,0.16), 0 3px 6px rgba(0,0,0,0.23);
}

.modal-footer {
  .btn.btn-primary {
    color: #fff !important;
    background-color: #9163CC !important;
  }
}
</style>
