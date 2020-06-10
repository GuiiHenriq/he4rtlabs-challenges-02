<template>
  <div class="main">
    <div class="row">
      <section class="actions">
        <button class="btn btn-primary btn-lg tooltip" data-tooltip="Adicionar" @click="actionStatus.modalFeature = !actionStatus.modalFeature"><i class="icon icon-plus"></i></button>
        <button class="btn btn-primary btn-lg tooltip" data-tooltip="Deletar" @click="actionStatus.activeDelete = !actionStatus.activeDelete"><i class="icon icon-delete"></i></button>
        <button class="btn btn-primary btn-lg tooltip" data-tooltip="Importar" @click="actionStatus.modalImport = !actionStatus.modalImport"><i class="icon icon-upload"></i></button>
        <button class="btn btn-primary btn-lg tooltip" data-tooltip="Exportar" @click="exportJson"><i class="icon icon-download"></i></button>
      </section>

      <section class="valor-hora">
        <div class="input-group">
          <span class="input-group-addon">R$</span>
          <input type="text" class="form-input" placeholder="Valor Hora" v-model.number.lazy="inputData.priceHour">
        </div>
      </section>
    </div>

    <div class="row content">
      <main>
        <h2>Suas Features</h2>

        <ul v-for="(item, index) in featureData" :key="index">
          <li>
            <strong>Funcionalidade:</strong> {{item.feature}}
            <button class="btn btn-action s-circle btn-sm" data="delete-item" :class="actionStatus.activeDelete ? 'active' : ''" @click="deleteItem(index)"><i class="icon icon-cross"></i></button>
          </li>
          <li><strong>Horas de Desenvolvimento:</strong> {{item.devHours}}h</li>
          <li><strong>Horas de Teste:</strong> {{item.qaHours}}h</li>
          <li><strong>Valor:</strong> {{item.pricePerFeature | numeroPreco}}</li>
        </ul>
        
        <!--<ul v-for="(item, index) in importFeatures" :key="`${index}-${item.id}`">
          <li>
            <strong>Funcionalidade:</strong> {{item.feature}}
            <button class="btn btn-action s-circle btn-sm" data="delete-item" :class="actionStatus.activeDelete ? 'active' : ''" @click="deleteItem(index)"><i class="icon icon-cross"></i></button>
          </li>
          <li><strong>Horas de Desenvolvimento:</strong> {{item.inputData.devHours}}h</li>
          <li><strong>Horas de Teste:</strong> {{item.qaHours}}h</li>
          <li><strong>Valor:</strong> {{item.pricePerFeature | numeroPreco}}</li>
        </ul>-->
      </main>

      <aside>
        <h6>Total de Funcionalidades: {{featureCounter.length}}</h6>
        <!--<ul>
          <li v-for="(feature, index) in features" :key="index">{{feature}}</li>
        </ul>-->
        <h6>Total Horas de Desenvolvimento: {{sumTotal.totalDevHours}}</h6>
        <h6>Total Horas de Teste: {{sumTotal.totalQaHours}}</h6>
        <h6>Valor Total: {{sumTotal.totalPriceHour | numeroPreco}}</h6>
      </aside>
    </div>

    <div v-if="actionStatus.modalFeature" class="modal" :class="actionStatus.modalFeature ? 'active' : ''" id="modal-id">
      <a @click.prevent="actionStatus.modalFeature = !actionStatus.modalFeature" href="#close" class="modal-overlay" aria-label="Close"></a>
      <div class="modal-container">
        <div class="modal-header">
          <a @click.prevent="actionStatus.modalFeature = !actionStatus.modalFeature" href="#close" class="btn btn-clear float-right" aria-label="Close"></a>
          <div class="modal-title h5">Insira Suas Features</div>
        </div>
        <div class="modal-body">
          <div class="content">
            <form @submit="addFeatures">
              <label class="form-label" for="features">Funcionalidade:</label>
              <input class="form-input" id="features" type="text" v-model.lazy="inputData.featureName" />

              <label class="form-label" for="dev-hours">Horas de Desenvolvimento:</label>
              <input class="form-input" id="dev-hours" type="number" v-model.number="inputData.devHours" />

              <label class="form-label" for="qa-hours">Horas de Teste:</label>
              <input class="form-input" id="qa-hours" type="number" v-model.number="inputData.qaHours" />
              <div class="modal-footer">
                <button class="btn btn-primary btn-lg"><i class="icon icon-check"></i></button>
              </div>
            </form>
          </div>
        </div>
      </div>
    </div>

    <div v-if="actionStatus.modalImport" class="modal" :class="actionStatus.modalImport ? 'active' : ''" id="modal-id">
      <a @click.prevent="actionStatus.modalImport = !actionStatus.modalImport" href="#close" class="modal-overlay" aria-label="Close"></a>
      <div class="modal-container">
        <div class="modal-header">
          <a @click.prevent="actionStatus.modalImport = !actionStatus.modalImport" href="#close" class="btn btn-clear float-right" aria-label="Close"></a>
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
      /*featureData: [
        {
        'id': 1,
        'feature': 'Correção BUG',
        'devHours': 5,
        'qaHours': 1.2
        }
      ],*/
      actionStatus: {
        modalFeature: false,
        modalImport: false,
        activeDelete: false,
      },
      inputData: {
        featureName: "",
        devHours: "",
        qaHours: "",
        priceHour: "",
      },
      sumTotal: {
        totalDevHours: "",
        totalQaHours: "",
        totalPriceHour: "",
      },
      featureData: [],
      featureExport: [],
      featureCounter: [],
    };
  },
  methods: {
    addFeatures(e) {
      e.preventDefault();
      // Inserindo os Dados do Form em um Objeto
      let obj;
      if (this.inputData.featureName.trim().length && this.inputData.priceHour) {
        obj = {
          //id: this.featureData.length + 1,
          feature: this.inputData.featureName,
          devHours: this.inputData.devHours,
          qaHours: this.inputData.qaHours,
          pricePerFeature: this.inputData.priceHour * (this.inputData.devHours + this.inputData.qaHours)
        };
        this.featureData.push(obj);
        this.featureExport.push(obj);

        this.inputData.featureName = "";
        this.inputData.devHours = "";
        this.inputData.qaHours = "";
        this.pricePerFeature = "";
      
        // Soma das Horas de DEV, QA e Contador de Features
        let sumDevHours = 0;
        let sumQAHours = 0;
        let sumFeatures = 0;
        let totalPriceHour = 0;
        for (let i = 0; i < this.featureData.length; i++) {
          sumFeatures = this.featureData[i].feature;
          sumDevHours += this.featureData[i].devHours;
          sumQAHours += this.featureData[i].qaHours;
          this.sumTotal.totalPriceHour = (totalPriceHour += this.featureData[i].pricePerFeature);
        }
        this.featureCounter.push(sumFeatures);
        this.sumTotal.totalDevHours = sumDevHours;
        this.sumTotal.totalQaHours = sumQAHours;

      } else {
        alert('Preencha seu valor por hora!')
      }

        // Soma de Todos os Valores
        /*let totalPriceHour = 0;
        for ( let i = 0; i < this.listPriceHour.length; i++ ){
          totalPriceHour += this.listPriceHour[i];
          console.log(this.listPriceHour[i])
        }
        this.sumTotal.totalPriceHour = totalPriceHour;*/
    },
    deleteItem(index) {
      if(this.featureData.length) {
        this.$delete(this.featureData, index);
        this.$delete(this.featureExport, index);
      }

      let sumDevHours = 0;
      let sumQAHours = 0;
      for (let i = 0; i < this.featureData.length; i++) {
        sumDevHours = this.featureData[i].devHours;
        sumQAHours = this.featureData[i].qaHours;
        this.sumTotal.totalPriceHour =  this.featureData[i].pricePerFeature;
      }
      this.featureCounter.splice(1, 1)
      this.sumTotal.totalDevHours = sumDevHours;
      this.sumTotal.totalQaHours = sumQAHours;

      if(this.featureData.length == 0) {
        this.featureCounter = [];
        this.sumTotal.totalPriceHour = 0;
      }
    },
    exportJson() {
      for (let i = 0; i < this.featureExport.length; i++) delete this.featureExport[i].pricePerFeature;
      let dataStr = "data:text/json;charset=utf-8," + encodeURIComponent(JSON.stringify(this.featureExport));
      let downloadAnchorNode = document.createElement("a");
      console.log(this.featureExport)
      if(this.featureData.length) {
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
      //console.log("selected a file");
      //console.log(this.$refs.myFile.files[0]);
      let file = this.$refs.myFile.files[0];
      if(!file || file.type !== 'application/json') return alert('TIPO DE ARQUIVO INCORRETO');

      let reader = new FileReader();
      reader.readAsText(file, "UTF-8");
      reader.onload = evt => {
        this.featureData = [];
        this.featureCounter = [];
        
        let result = JSON.parse(evt.target.result);
        if(!result[0].feature) return alert('PADRÃO DO JSON INCORRETO');
        let obj;
        let sumDevHours = 0;
        let sumQAHours = 0;
        let totalPriceHour = 0;
        if(this.inputData.priceHour) {
          for (let i = 0; i < result.length; i++) {
            // Inserindo os Dados do Json em um Objeto
            obj = {
              //id: result[i].length + 1,
              feature: result[i].feature,
              devHours: result[i].devHours,
              qaHours: result[i].qaHours,
              pricePerFeature: this.inputData.priceHour * (result[i].devHours + result[i].qaHours)
            };
            this.featureData.push(obj);
            this.featureExport.push(obj);

            // Soma das Horas de DEV, QA e Contador de Features
            this.featureCounter.push(result[i].feature);
            this.sumTotal.totalDevHours = (sumDevHours += result[i].devHours);
            this.sumTotal.totalQaHours = (sumQAHours += result[i].qaHours);

            // Soma de Todos os Valores
            this.sumTotal.totalPriceHour = (totalPriceHour += this.featureData[i].pricePerFeature);
          }
        } else {
          alert('Preencha seu valor por hora!');
          this.actionStatus.modalImport = false;
        }
      };
      reader.onerror = evt => {
        console.error(evt);
      };
      this.actionStatus.modalImport = false;
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
