<template>
  <footer class="navbar">
    <section class="navbar-section">
      <img src="../assets/logo.png" class="img-responsive ..." alt="...">
    </section>
    <section class="navbar-center">
     <h1>Footer</h1>
    </section>
    <section class="navbar-section">
    </section>
  </footer>
</template>

<script>
export default {
  name: 'Footer'
}
</script>

<style lang="scss" scoped>
@import url('https://fonts.googleapis.com/css2?family=Anton&display=swap');

footer {
  background-color: #0d1317;
  border-bottom-left-radius: 5px;
  border-bottom-right-radius: 5px;
  color: #fff;

  img {
    max-width: 200px;
  }

  h1 {
    font-family: 'Anton', sans-serif;
  }
}
</style>
