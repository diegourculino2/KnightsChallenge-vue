<template>
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@4.6.0/dist/css/bootstrap.min.css" />
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.11.3/font/bootstrap-icons.min.css">
  <div id="app" class="container my-3">

 
      <div class="row">
        <div class="col">
          <img width="220" src="./assets/logo.png">
        </div>
        <div class="col">
          <h4>Diego Urçulino de Sousa</h4>
          <p>Teste Técnico - BTG Pactual</p>
        </div>
      </div>


      <div class="row acoes-header">
        <div class="col">
          <button type="button" class="btn btn-success" @click="showModal = true; knightSaveSucess = false">
            <i class="bi bi-plus-circle"></i>
            Adicionar Knights
          </button>
        </div>
        <div class="col d-flex flex-row-reverse">
          <button type="button" @click="getHeroes()" class="btn btn-warning">
            Hall Of Heroes
            <i class="bi bi-star-fill"></i>
          </button>
        </div>
      </div>

      <div v-if="knightSaveSucess" class="alert alert-success alert-dismissible fade show margin-alert" role="alert">
        <strong>Sucesso!</strong>
        <button @click="knightSaveSucess = false" type="button" class="close" data-dismiss="alert" aria-label="Close">
          <span aria-hidden="true">&times;</span>
        </button>
      </div>

      <div v-if="knightSaveError" class="alert alert-danger alert-dismissible fade show margin-alert" role="alert">
        <strong>Erro ao salvar Knight!</strong>
        <button @click="knightSaveSucess = false" type="button" class="close" data-dismiss="alert" aria-label="Close">
          <span aria-hidden="true">&times;</span>
        </button>
      </div>

      <div v-if="loadListKnights" class="loader">
          <div class="spinner-grow text-primary margin-loader" v-for="index in contentLoad" :key="index" role="status"></div>
      </div>

      <div v-if="loadListKnights == false && knightsResult == null" class="cards-render">
        <div class="card margin-card">
          <h3 class="h3-padding">Nenhum knight cadastrado</h3>
        </div>
      </div>

      <div v-if="loadListKnights == false && knightsResult != null" class="cards-render">
        <div class="card margin-card" v-for="(knight, index) in knightsResult" :key="index">
          <div class="card-body">
            <div class="row">
              <div class="col">
                <h5 class="card-title">{{ knight.name }}</h5>
                <span class="badge badge-primary padding-badge">
                  <i class="bi bi-person-fill"></i>
                  Idade: 25
                </span>
                <span class="badge badge-dark padding-badge">
                  <i class="bi bi-shield-fill"></i>
                  Armas: 2
                </span>
              </div>
              <div class="col d-flex flex-row-reverse">
                <div row>
                  <button type="button" class="btn btn-primary btn-lg" @click="getSingleKnight(knight.id)">
                    <i class="bi bi-pencil-square"></i>
                  </button>
                  <button type="button" class="btn btn-danger btn-lg margin-buttons" @click="knightDelete(knight.id)">
                    <i class="bi bi-trash"></i>
                  </button>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>

      <!-- Modal -->
      <modal
        @submitKnight="knightSubmission"
        :show="showModal" :salvando="salvando" @close="showModal = false" @save="saveKnight">
      </modal>

      <!-- ModalHeroes -->
      <modal-heroes
        :show="showModalHeroes" :listHeroes="listHeroes" @close="showModalHeroes = false">
      </modal-heroes>

      <!-- ModalSingleKnight -->
      <modal-single-knight
        @editKnightEvent="knightEdtion"
        :show="showModalSingleKnight" :singleKnight="singleKnight" :editando="editando" @close="showModalSingleKnight = false">
      </modal-single-knight>
 
  </div>
</template>

<script>
import http from "./http-common";
import Modal from './NewKnight.vue';
import ModalHeroes from './Heroes.vue';
import ModalSingleKnight from './SingleKnight.vue';


export default {
  name: "App",
  data() {
    return {
      contentLoad: null,
      loadListKnights: false,
      showModal: false,
      showModalHeroes: false,
      showModalSingleKnight: false,
      knightsResult: null,
      salvando: false,
      editando: false,
      knightSaveSucess: false,
      knightSaveError: false,
      listHeroes: null,
      singleKnight: null
    }
  },
  components: {
    Modal,
    ModalHeroes,
    ModalSingleKnight
  },
  methods: {
    async getAllKnights() {
      this.contentLoad = 10;
      this.loadListKnights = true;
      try {
        setTimeout(() => {
          this.loadListKnights = false;
        }, "2000");
        const res = await http.get("/knights");

        const result = {
          status: res.status + "-" + res.statusText,
          headers: res.headers,
          data: res.data,
        };

        this.knightsResult = null;
        this.knightsResult = result.data;
      } catch (err) {
        this.knightsResult = this.fortmatResponse(err.response?.data) || err;
      }
    },

    async getHeroes() {
      this.listHeroes = null;
      try {
        const res = await http.get("/knights/filter=hero");

        const result = {
          status: res.status + "-" + res.statusText,
          headers: res.headers,
          data: res.data,
        };
        this.listHeroes = result.data;
        this.showModalHeroes = true;
      } catch (err) {
        this.listHeroes = this.fortmatResponse(err.response?.data) || err;
      }
    },

    knightSubmission(newKnight) {
      this.salvando = true;
        const res = http.post("/knights", newKnight);
        console.log(res)

        setTimeout(() => {
          this.salvando = false;
          this.showModal = false;
          this.knightSaveSucess = true;
          this.getAllKnights();
        }, "2000");
    },

    knightEdtion(newKnight){
      const newNickname = { newNickname: newKnight.nickName}
      this.editando = true;
      const res = http.put(`/knights/${newKnight.id}`, newNickname);
        console.log(res)

        setTimeout(() => {
          this.editando = false;
          this.showModalSingleKnight = false;
          this.knightSaveSucess = true;
          this.getAllKnights();
        }, "2000");
    },

    knightDelete(id){
      this.contentLoad = 10;
      this.loadListKnights = true;
      this.knightsResult = null;
      this.knightSaveSucess = false;
      const res = http.delete(`/knights/${id}`);
      console.log(res)
      setTimeout(() => {
          this.getAllKnights();
        }, "2000");
    },

    async getSingleKnight(id){
      this.knightSaveSucess = false;
      this.singleKnight = null;
      try {
        const res = await http.get(`/knights/${id}`);
        const result = {
          status: res.status + "-" + res.statusText,
          headers: res.headers,
          data: res.data,
        };
        this.singleKnight = result.data;
        this.showModalSingleKnight = true;
      } catch (err) {
        this.singleKnight = this.fortmatResponse(err.response?.data) || err;
      }
    },

    async saveKnight(){

    }

  },
  beforeMount() {
    this.getAllKnights()
  }
}
</script>

<style>
#app {
  max-width: 100vw;
  margin: auto;
}
h4, p{
  text-align: end;
  padding-right: 20px;
}

.acoes-header{
  padding: 30px 20px;
}

.margin-card{
  margin: 0px 20px 30px 20px;
}

.margin-buttons{
  margin: 0px 20px;
}
.margin-alert{
  margin: 0px 20px;
}
.h3-padding{
  padding: 20px;
}

.loader{
  width: 100%;
  text-align: center;
}

.margin-loader{
  margin: 20px 10px;
}
.padding-badge{
  i{
    margin-right: 5px;
  }
  padding: 10px !important;
  margin-left: 8px;
  font-weight: normal !important;
}

</style>