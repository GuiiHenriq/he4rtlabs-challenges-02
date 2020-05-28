<template>
  <div class="main">
    <div class="row">
      <section class="actions">
        <button>Inserir</button>
        <button>Apagar</button>
        <button>Importar</button>
        <button @click="createJson">Exportar</button>
      </section>

      <section class="valor-hora">
        <label for="price-hour">Valor Hora:</label>
        <input id="price-hour" type="text" v-model="priceHour" />
      </section>
    </div>

    <div class="row">
      <main>
        <ul v-for="item in todos" :key="item.id">
          <li>Funcionalidade: {{item.feature}}</li>
          <li>Horas de Desenvolvimento: {{item.devHours}}</li>
          <li>Horas de Teste: {{item.qaHours}}</li>
        </ul>
      </main>

      <aside>
        <h2>Funcionalidades:{{feature}}</h2>
        <h2>Horas de Desenvolvimento:{{totalDevHours}}</h2>
        <h2>Horas de Teste:{{totalQaHours}}</h2>
        <h2>Valor Total:{{priceHour}}</h2>
      </aside>
    </div>

    <div class="form">
      <form @submit="handleSubmit">
        <label for="features">Funcionalidade:</label>
        <input id="features" type="text" v-model.lazy="feature" />

        <label for="dev-hours">Horas de Desenvolvimento:</label>
        <input id="dev-hours" type="number" v-model.number="devHours" />

        <label for="qa-hours">Horas de Teste:</label>
        <input id="qa-hours" type="number" v-model.number="qaHours" />
        <button>Adicionar</button>
      </form>
    </div>
  </div>
</template>

<script>
export default {
  name: "Main",
  data() {
    return {
      priceHour: 0,
      objFeature: null,
      input: 1,
      /*todos: [{
        'id': 1,
        'feature': 'name 1',
        'devHours': 'devHours 1',
        'qaHours': 'qaHours 1'
      }],*/
      todos: [],
      feature: '',
      devHours: '',
      qaHours: '',
      totalDevHours: '',
      totalQaHours: ''
    };
  },
  methods: {
    handleSubmit: function(e) {
      e.preventDefault();
      let obj;
      if (this.feature.trim().length) {
        obj = {
          'id': this.todos.length + 1,
          'feature': this.feature,
          'devHours': this.devHours,
          'qaHours': this.qaHours
        }
        this.todos.push(obj);
        this.feature = '';
        this.devHours = '';
        this.qaHours = '';
      }

      let sumDevHours = 0;
      let sumQAHours = 0;
      for (let i = 0; i < this.todos.length; i++) {
        sumDevHours = sumDevHours + this.todos[i].devHours;
        sumQAHours = sumQAHours + this.todos[i].qaHours;
      }
      this.totalDevHours = sumDevHours;
      this.totalQaHours = sumQAHours;
    },
    createJson() {
      const jsonExport = {
        feature: this.features,
        devHours: this.devHours,
        qaHours: this.qaHours
      };
      console.log(jsonExport);
    }
  },
};
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

  main {
    min-width: 70%;
    background: #222f3e;
    color: white;
  }

  aside {
    background: #576574;
    color: white;
    min-width: 30%;
  }
}

.form {
  width: 50%;
  margin: 0 auto;
  form {
    display: flex;
    flex-direction: column;
  }
}
</style>
