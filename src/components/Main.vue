<template>
  <div class="main">
    <div class="row">
      <section class="actions">
        <button class="btn btn-primary btn-lg tooltip" data-tooltip="Adicionar" @click="modalFeature = !modalFeature"><i class="icon icon-plus"></i></button>
        <button class="btn btn-primary btn-lg tooltip" data-tooltip="Deletar" @click="activeDelete = !activeDelete"><i class="icon icon-delete"></i></button>
        <button class="btn btn-primary btn-lg tooltip" data-tooltip="Importar" @click="modalImport = !modalImport"><i class="icon icon-upload"></i></button>
        <button class="btn btn-primary btn-lg tooltip" data-tooltip="Exportar" @click="exportJson"><i class="icon icon-download"></i></button>
      </section>

      <section class="valor-hora">
        <div class="input-group">
          <span class="input-group-addon">R$</span>
          <input type="text" class="form-input" placeholder="Valor Hora" v-model.number.lazy="priceHour">
        </div>
      </section>
    </div>

    <div class="row content">
      <main>
        <h2>Suas Features</h2>
        <ul v-for="(item, index) in todos" :key="item.id">
          <li>
            <strong>Funcionalidade:</strong> {{item.feature}}
            <button class="btn btn-action s-circle btn-sm" data="delete-item" :class="activeDelete ? 'active' : ''" @click="deleteItem(index)"><i class="icon icon-cross"></i></button>
          </li>
          <li><strong>Horas de Desenvolvimento:</strong> {{item.devHours}}h</li>
          <li><strong>Horas de Teste:</strong> {{item.qaHours}}h</li>
          <li><strong>Valor:</strong> {{item.pricePerFeature | numeroPreco}}</li>
        </ul>

        <ul v-for="(item, index) in importFeatures" :key="item.id">
          <li>
            Funcionalidade: {{item.feature}}
            <button class="btn btn-action s-circle btn-sm" data="delete-item" :class="activeDelete ? 'active' : ''" @click="deleteItem(index)"><i class="icon icon-cross"></i></button>
          </li>
          <li>Horas de Desenvolvimento: {{item.devHours}}</li>
          <li>Horas de Teste: {{item.qaHours}}</li>
        </ul>
      </main>

      <aside>
        <h6>Total de Funcionalidades: {{features.length}}</h6>
        <!--<ul>
          <li v-for="(feature, index) in features" :key="index">{{feature}}</li>
        </ul>-->
        <h6>Total Horas de Desenvolvimento: {{totalDevHours}}</h6>
        <h6>Total Horas de Teste: {{totalQaHours}}</h6>
        <h6>Valor Total: {{totalPriceHour | numeroPreco}}</h6>
      </aside>
    </div>

    <div v-if="modalFeature" class="modal" :class="modalFeature ? 'active' : ''" id="modal-id">
      <a @click.prevent="modalFeature = !modalFeature" href="#close" class="modal-overlay" aria-label="Close"></a>
      <div class="modal-container">
        <div class="modal-header">
          <a @click.prevent="modalFeature = !modalFeature" href="#close" class="btn btn-clear float-right" aria-label="Close"></a>
          <div class="modal-title h5">Insira Suas Features</div>
        </div>
        <div class="modal-body">
          <div class="content">
            <form @submit="handleSubmit">
              <label class="form-label" for="features">Funcionalidade:</label>
              <input class="form-input" id="features" type="text" v-model.lazy="feature" />

              <label class="form-label" for="dev-hours">Horas de Desenvolvimento:</label>
              <input class="form-input" id="dev-hours" type="number" v-model.number="devHours" />

              <label class="form-label" for="qa-hours">Horas de Teste:</label>
              <input class="form-input" id="qa-hours" type="number" v-model.number="qaHours" />
              <div class="modal-footer">
                <button class="btn btn-primary btn-lg"><i class="icon icon-check"></i></button>
              </div>
            </form>
          </div>
        </div>
      </div>
    </div>

    <div v-if="modalImport" class="modal" :class="modalImport ? 'active' : ''" id="modal-id">
      <a @click.prevent="modalImport = !modalImport" href="#close" class="modal-overlay" aria-label="Close"></a>
      <div class="modal-container">
        <div class="modal-header">
          <a @click.prevent="modalImport = !modalImport" href="#close" class="btn btn-clear float-right" aria-label="Close"></a>
          <div class="modal-title h5">Importe Seu JSON</div>
        </div>
        <div class="modal-body">
          <div class="content">
            <input class="form-file" type="file" ref="myFile" @change="importJson" />
          </div>
        </div>
      </div>
    </div>

  </div>
</template>

<script>
export default {
  name: "Main",
  data() {
    return {
      todos: [{
        'id': 1,
        'feature': 'Correção BUG',
        'devHours': 5,
        'qaHours': 1.2
      }],
      modalFeature: false,
      modalImport: false,
      activeDelete: false,
      //todos: [],
      feature: "",
      devHours: "",
      qaHours: "",
      priceHour: "",
      totalDevHours: "",
      totalQaHours: "",
      totalPriceHour: "",
      listPriceHour: [],
      features: [],
      importFeatures: []
    };
  },
  methods: {
    handleSubmit(e) {
      e.preventDefault();

      // Inserindo os Dados do Form em um Objeto
      let obj;
      if (this.feature.trim().length && this.priceHour) {
        obj = {
          id: this.todos.length + 1,
          feature: this.feature,
          devHours: this.devHours,
          qaHours: this.qaHours,
          pricePerFeature: this.priceHour * (this.devHours + this.qaHours)
        };
        this.todos.push(obj);
        this.feature = "";
        this.devHours = "";
        this.qaHours = "";
        this.pricePerFeature = "";
      } else {
        alert('Preencha seu valor por hora!')
      }

      // Soma das Horas de DEV, QA e Contador de Features
      let sumDevHours = 0;
      let sumQAHours = 0;
      let sumFeatures = 0;
      let sumPriceHour = 0;
      for (let i = 0; i < this.todos.length; i++) {
        sumFeatures = this.todos[i].feature;
        sumDevHours = sumDevHours + this.todos[i].devHours;
        sumQAHours = sumQAHours + this.todos[i].qaHours;
        sumPriceHour = this.todos[i].pricePerFeature;
      }
      this.listPriceHour.push(sumPriceHour);
      this.features.push(sumFeatures);
      this.totalDevHours = sumDevHours;
      this.totalQaHours = sumQAHours;

      // Soma de Todos os Valores
      let totalPriceHour = 0;
      for ( let i = 0; i < this.listPriceHour.length; i++ ){
        totalPriceHour += this.listPriceHour[i];
      }
      this.totalPriceHour = totalPriceHour;
    },
    deleteItem(index) {
      if(this.todos.length) {
        this.$delete(this.todos, index);
      }
      if(this.importFeatures.length) {
        this.$delete(this.importFeatures, index);
      }
    },
    exportJson() {
      let dataStr = "data:text/json;charset=utf-8," + encodeURIComponent(JSON.stringify(this.todos));
      let downloadAnchorNode = document.createElement("a");
      console.log(this.todos)
      if(this.todos.length) {
        downloadAnchorNode.setAttribute("href", dataStr);
        downloadAnchorNode.setAttribute("download", "features" + ".json");
        document.body.appendChild(downloadAnchorNode);
        downloadAnchorNode.click();
        downloadAnchorNode.remove();
      } else {
        alert('Preencha!')
      }
    },
    importJson() {
      console.log("selected a file");
      console.log(this.$refs.myFile.files[0]);

      let file = this.$refs.myFile.files[0];
      //if(!file || file.type !== 'text/plain') return;

      let reader = new FileReader();
      reader.readAsText(file, "UTF-8");
      reader.onload = evt => {
        //this.text = evt.target.result;
        let result = JSON.parse(evt.target.result);
        for (let i = 0; i < result.length; i++) {
          this.importFeatures.push(result[i]);
        }
      };
      reader.onerror = evt => {
        console.error(evt);
      };
      this.modalImport = false;
    }
  },
  beforeUpdate() {
    //this.totalPriceHour = this.priceHour * (this.totalDevHours + this.totalQaHours);
    //console.log(`Price Hour ${this.priceHour}`);
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
    min-width: 65%;
    background: #fff;
    color: #0d1317;
    box-shadow: 0 1px 3px rgba(0,0,0,0.12), 0 1px 2px rgba(0,0,0,0.24);
    transition: all 0.3s cubic-bezier(.25,.8,.25,1);
  }

  aside {
    background: #6564db;
    color: white;
    min-width: 30%;
    box-shadow: 0 1px 3px rgba(0,0,0,0.12), 0 1px 2px rgba(0,0,0,0.24);
    transition: all 0.3s cubic-bezier(.25,.8,.25,1);
  }
}

.content {
  padding-top: 10px;
}

form {
  display: flex;
  flex-direction: column;
}

.btn[data="delete-item"] {
  display: none;
}

.btn[data="delete-item"].active {
  display: inline-block;
}
</style>
