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
        <ul>
          <li v-for="(valor, index) in objFeature" :key="index">
            <p>Funcionalidade: {{valor.feature}}</p>
            <p>Horas de Desenvolvimento: {{valor.devHours}}</p>
            <p>Horas de Teste: {{valor.qaHours}}</p>
          </li>
        </ul>
      </main>

      <aside>
        <h2>Funcionalidades:{{features}}</h2>
        <h2>Horas de Desenvolvimento:{{devHours}}</h2>
        <h2>Horas de Teste:{{qaHours}}</h2>
        <h2>Valor Total:{{priceHour}}</h2>
      </aside>
    </div>

    <div class="form">
      <form v-for="i in input" :key="i" type="text" class="form-control" :id='"item"+i'>
        <label for="features">Funcionalidade:</label>
        <input id="features" type="text" v-model.lazy="features" />

        <label for="dev-hours">Horas de Desenvolvimento:</label>
        <input id="dev-hours" type="number" v-model.lazy.number="devHours" class="test" />

        <label for="qa-hours">Horas de Teste:</label>
        <input id="qa-hours" type="number" v-model.lazy.number="qaHours" />
      </form>
      <!--<form v-for="i in input" :key="i" type="text" class="form-control" :id='"item"+i'>
        <label for="features">Funcionalidade:</label>
        <input id="features" type="text" class="features" />

        <label for="dev-hours">Horas de Desenvolvimento:</label>
        <input id="dev-hours" type="number" class="dev-hours" />

        <label for="qa-hours">Horas de Teste:</label>
        <input id="qa-hours" type="number" class="qa-hours" />
      </form>-->
      
      <button @click="insertData">Inserir</button>
      <button @click="input++" type="button">Adicionar</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "Main",
  data() {
    return {
      priceHour: 0,
      features: "",
      devHours: 0,
      qaHours: 0,
      objFeature: null,
      input: 1
    };
  },
  methods: {
    insertData() {
      this.objFeature = [
        {
          feature: this.features,
          devHours: this.devHours,
          qaHours: this.qaHours
        }
      ];
      return this.objFeature;

      /*const elFeatures = document.querySelectorAll('.features');
      const elDevHours = document.querySelector('.dev-hours').value;
      const elQAHours = document.querySelector('.qa-hours').value;
      for (let i = 0; i < elFeatures.length; i++) {
        this.objFeature = [
          {
            feature: elFeatures[i].value
          }
        ]
      }*/
    },
    createJson() {
      const jsonExport = {
        feature: this.features,
        devHours: parseFloat(this.devHours),
        testHours: parseFloat(this.qaHours)
      };
      console.log(jsonExport);
    }
  }
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
