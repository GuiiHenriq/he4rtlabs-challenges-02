<template>
  <header class="navbar">
    <section class="navbar-section">
      <img src="../assets/logo.png" class="img-responsive" alt="Logo He4rt devs">
    </section>
    <section class="navbar-center">
     <h1>Planilha Freelas</h1>
    </section>
    <section class="navbar-section">
    </section>
  </header>
</template>

<script>

export default {
  name: "Header"
};
</script>

<style lang="scss" scoped>
@import url('https://fonts.googleapis.com/css2?family=Anton&display=swap');

header {
  border-top-left-radius: 5px;
  border-top-right-radius: 5px;
  color: #fff;

  img {
    max-width: 200px;
  }

  h1 {
    font-family: 'Anton', sans-serif;
    color: black;
  }
}
</style>
