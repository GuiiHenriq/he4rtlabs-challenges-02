<template>
  <div class="main">
    <div class="row">
      <section class="actions">
        <button>Inserir</button>
        <button>Apagar</button>
        <button>Importar</button>
        <button>Exportar</button>
      </section>

      <section class="valor-hora">
        <input type="value">
      </section>
    </div>

    <div class="row">
      <main><p>List Funcionalidades</p></main>
      <aside><p>Valores</p></aside>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Main'
}
</script>

<style lang="scss" scoped>
.main {
  padding: 30px 0;
}
.row {
  display: flex;
  flex-direction: row;
  width: 100%;
  justify-content: space-between;

  section {
  }
  
  main {
    min-width: 70%;
    background: red;
  }

  aside {
    background: blueviolet;
    min-width: 30%;
  }
}
</style>
