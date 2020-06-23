<template>
  <div class="main">
    <div class="row head">
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

        <ul>
          <div class="list-head desk">
            <li><strong>Funcionalidade</strong></li>
            <li><strong>Horas Dev</strong></li>
            <li><strong>Horas Teste</strong></li>
            <li><strong>Valor</strong></li>
          </div>
          <div class="list-head mobile">
            <li><strong>Feature</strong></li>
            <li><strong>Dev</strong></li>
            <li><strong>Teste</strong></li>
            <li><strong>Valor</strong></li>
          </div>
          <li v-for="(item, index) in featureData" :key="index">
            <p>{{item.feature}}</p>
            <p>{{item.devHours}}h</p>
            <p>{{item.qaHours}}h</p>
            <p>{{item.pricePerFeature | numeroPreco}}</p>
            <button class="btn" data="delete-item" :class="actionStatus.activeDelete ? 'active' : ''" @click="deleteItem(index)"><i class="icon icon-cross"></i></button>
          </li>
        </ul>
      </main>

      <aside>
        <h2>Resumo</h2>

        <div>
          <h6>Total de Funcionalidades: <strong>{{featureCounter.length}}</strong></h6>
          <h6>Total Horas Desenvolvimento: <strong>{{sumTotal.totalDevHours}}</strong></h6>
          <h6>Total Horas Teste: <strong>{{sumTotal.totalQaHours}}</strong></h6>
          <h6>Valor Total: <strong>{{sumTotal.totalPriceHour | numeroPreco}}</strong></h6>
        </div>
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
@import url('https://fonts.googleapis.com/css2?family=Oswald:wght@300&display=swap');

.main {
  padding: 30px 0;
}

.row {
  display: flex;
  flex-direction: row;
  width: 100%;
  justify-content: space-between;

  h2 {
    font-family: 'Oswald', sans-serif;
    color: #000;
  }
  
  &.head {
    padding-bottom: 10px;
    border-bottom: 2px solid #D8DAE6;

    .actions {
        button {
          margin: 0 5px;
        }
      }

    .valor-hora {
      width: 30%;
    }
  }
  
  main {
    min-width: 65%;
    background: transparent;
    color: #0d1317;

    ul {
      overflow: auto;
      margin: 0 !important;
    }

    .list-head {
      display: flex;
      margin-bottom: 15px;
      background: transparent;
      //box-shadow: 0 4px 6px 0 rgba(31,70,88,.04);
      font-family: 'Roboto', sans-serif;
      font-weight: 300;

      li {
        display: flex;
        justify-content: space-around;
        flex-direction: row;
        width: 100%;
        margin: 0 !important;
        color: #D8DAE6;
        border-radius: 0;
        text-transform: uppercase;
        letter-spacing: -0.8px;
      }
    }

    li {
      //box-shadow: 0 1px 3px rgba(0,0,0,0.12), 0 1px 2px rgba(0,0,0,0.24);
      //box-shadow: 0 4px 6px 0 rgba(31,70,88,.04);
      transition: all 0.3s cubic-bezier(.25,.8,.25,1);
      width: 100%;
      margin: 0 0 3px 0;
      padding: 10px;
      background: #fff;
      font-size: 18px;
      display: flex;
      justify-content: space-around;
      border-radius: 5px;

      p {
        width: 100%;
        text-align: center;
        margin: 5px 0;
      }
    }
  }

  aside {
    min-width: 30%;
    display: flex;
    flex-direction: column;

    div {
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      border-radius: 5px;
      padding: 20px 0;
      background: #9163CC;
      box-shadow: 0 1px 3px rgba(0,0,0,0.12), 0 1px 2px rgba(0,0,0,0.24);
      transition: all 0.3s cubic-bezier(.25,.8,.25,1);
      color: white;
      height: 100%;
    }
  }
}

.row.content {
  padding-top: 25px;
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
  color: #fff !important;
  background-color: #9163CC !important;
  border: none !important;
}

.modal-container {
  .modal-footer {
    padding: .8rem 0;
  }
}

.list-head.desk {display: flex !important}
.list-head.mobile {display: none !important}

// Mobile
@media only screen and (max-width: 768px) {
  .list-head.desk {display: none !important}
  .list-head.mobile {display: flex !important}

  .row {
    flex-direction: column;

    &.head {
      flex-wrap: wrap-reverse;

      .actions {
      display: flex;
      justify-content: center;

        button {
          margin: 0 10px 5px 10px;
        }
      }
      
      .valor-hora {
        width: 100%;
      }
    }

    aside {
      margin-top: 25px;
    }
  }
}
</style>
