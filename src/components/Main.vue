<template>
  <div class="main">
    <div class="row">
      <section class="actions">
        <button>Inserir</button>
        <button>Apagar</button>
        <button>Importar</button>
        <button @click="exportJson">Exportar</button>
      </section>

      <section class="valor-hora">
        <label for="price-hour">Valor Hora:</label>
        <input id="price-hour" type="text" v-model.number="priceHour" />
      </section>
    </div>

    <div class="row">
      <main>
        <ul v-for="item in todos" :key="item.id">
          <li @click="apagar">{{item.id}}<li>
          <li>Funcionalidade: {{item.feature}}</li>
          <li>Horas de Desenvolvimento: {{item.devHours}}</li>
          <li>Horas de Teste: {{item.qaHours}}</li>
        </ul>

        <ul v-for="(item, index) in importFeatures" :key="index">
          <li>Funcionalidade: {{importFeatures}}</li>
        </ul>

        <h1>{{teste}}</h1>
      </main>

      <aside>
        <h2>Funcionalidades:</h2>
        <ul>
          <li v-for="(feature, index) in features" :key="index">
            {{feature}}
          </li>
        </ul>
        <h2>Horas de Desenvolvimento: {{totalDevHours}}</h2>
        <h2>Horas de Teste: {{totalQaHours}}</h2>
        <h2>Valor Total: {{totalPriceHour}}</h2>
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

    <input type="file" id="fileInput">
  </div>
</template>

<script>
export default {
  name: "Main",
  data() {
    return {
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
      priceHour: '',
      totalDevHours: '',
      totalQaHours: '',
      totalPriceHour: '',
      features: [],
      importFeatures: [],
      teste: ''
    };
  },
  methods: {
    handleSubmit(e) {
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
      let teste;
      for (let i = 0; i < this.todos.length; i++) {
        teste = this.todos[i].feature;
        sumDevHours = sumDevHours + this.todos[i].devHours;
        sumQAHours = sumQAHours + this.todos[i].qaHours;
      }
      console.log(teste)
      this.features.push(teste);
      this.totalDevHours = sumDevHours;
      this.totalQaHours = sumQAHours;
      this.totalPriceHour = this.priceHour * (this.totalDevHours + this.totalQaHours);
    },
    apagar(e) {
      console.log(e.event)
    },
    createJson() {
      console.log(this.todos);
    },
    exportJson(){
      let dataStr = "data:text/json;charset=utf-8," + encodeURIComponent(JSON.stringify(this.todos));
      let downloadAnchorNode = document.createElement('a');
      downloadAnchorNode.setAttribute("href",     dataStr);
      downloadAnchorNode.setAttribute("download", "features" + ".json");
      document.body.appendChild(downloadAnchorNode);
      downloadAnchorNode.click();
      downloadAnchorNode.remove();
    },
    importJson() {
      const upload = document.getElementById('fileInput');
        if (upload) {
          upload.addEventListener('change', function() {
            if (upload.files.length > 0) {
              let reader = new FileReader();
              
              reader.addEventListener('load', function() {
                let result = JSON.parse(reader.result);
                
                for (let i = 0; i < result.length; i++) {
                  //let counter = result[i];
                  this.importFeatures = result[i];
                  this.teste = result[i].feature;
                  console.log(this.importFeatures)
                  console.log(this.teste)
                }
              });
              
              reader.readAsText(upload.files[0]);
            }
          });
        }
    }
  },
  created() {
    this.importJson();
  },
  mounted() {
    this.importJson();
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
