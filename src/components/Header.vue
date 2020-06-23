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
@import url('https://fonts.googleapis.com/css2?family=Oswald:wght@300&display=swap');

header {
  border-top-left-radius: 8px;
  border-top-right-radius: 8px;
  background-color: #fff;
  color: #0d1317;
  box-shadow: 0 1px 3px rgba(0,0,0,0.12), 0 1px 2px rgba(0,0,0,0.24);

  img {
    max-width: 200px;
  }

  h1 {
    font-family: 'Oswald', sans-serif;
    color: black;
  }
}

// Mobile
@media only screen and (max-width: 768px) {
  .navbar-section {
    justify-content: center;
  }
  .navbar-center {
    width: 100%;
    justify-content: center;
  }
}
</style>
