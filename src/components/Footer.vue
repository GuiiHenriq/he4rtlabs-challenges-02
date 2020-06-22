<template>
  <footer class="navbar">
    <section class="navbar-section">
    </section>
    <section class="navbar-center">
      <img src="../assets/logo-footer.png" class="img-responsive ..." alt="...">
    </section>
    <section class="navbar-section">
      <div>
        <a href="https://github.com/GuiiHenriq" target="_blank"><i class="icon icon-people"></i></a>
        <a href="https://www.linkedin.com/in/guilhermeerba/" target="_blank"><i class="icon icon-link"></i></a>
      </div>
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
  border-bottom-left-radius: 8px;
  border-bottom-right-radius: 8px;
  background-color: #fff;
  color: #0d1317;
  box-shadow: 0 1px 3px rgba(0,0,0,0.12), 0 1px 2px rgba(0,0,0,0.24);

  img {
    max-width: 200px;
  }

  h1 {
    font-family: 'Anton', sans-serif;
  }

  .navbar-section {
    margin-right: 0.5rem;

    div {
      margin-right: 2rem;

      a:first-child {
        padding: 0 1.125rem;
      }
    }
  }
}

@media only screen and (max-width: 768px) {
  .navbar-section {
    div {
      margin-right: 0 !important;
    }
  }
}
</style>
