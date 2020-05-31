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

        <ul v-for="item in importFeatures" :key="item.id">
          <li>Funcionalidade: {{item.feature}}</li>
          <li>Horas de Desenvolvimento: {{item.devHours}}</li>
          <li>Horas de Teste: {{item.qaHours}}</li>
        </ul>
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

    <input type="file" ref="myFile" @change="importJson">
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
      importFeatures: []
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
      let features;
      for (let i = 0; i < this.todos.length; i++) {
        features = this.todos[i].feature;
        sumDevHours = sumDevHours + this.todos[i].devHours;
        sumQAHours = sumQAHours + this.todos[i].qaHours;
      }
      this.features.push(features);
      this.totalDevHours = sumDevHours;
      this.totalQaHours = sumQAHours;
      this.totalPriceHour = this.priceHour * (this.totalDevHours + this.totalQaHours);
    },
    apagar(e) {
      console.log(e.event)
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
      console.log('selected a file');
      console.log(this.$refs.myFile.files[0]);
      
      let file = this.$refs.myFile.files[0];
      //if(!file || file.type !== 'text/plain') return;
      
      let reader = new FileReader();
      reader.readAsText(file, "UTF-8");
      reader.onload =  evt => {
        //this.text = evt.target.result;
        let result = JSON.parse(evt.target.result);
        for (let i = 0; i < result.length; i++) {
          this.importFeatures.push(result[i]);
        }
        
      }
      reader.onerror = evt => {
        console.error(evt);
      }
      
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
