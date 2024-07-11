<script>
import moment from 'moment';
export default {
    data() {
        return {
            newBirthday: "",
            knight: [],
            errorForm: false,
            name: "",
            nickname: "",
            birthday: "",
            weapons:{
              name: "",
              mod: 0,
              attr: "",
              equipped: true
            },
            attributes: {
              strength: 0,
              dexterity: 0,
              constitution: 0,
              intelligence: 0,
              wisdom: 0,
              charisma: 0
            },
            keyAttribute: ""
        }
  },
  props: {
    show: Boolean,
    salvando: Boolean
  },
  methods: {
    submitForm() {
        if(this.knight[0].name != null && this.knight[0].name != '' ){
            this.errorForm = false;
            const newKnight = JSON.parse(JSON.stringify(this.knight[0]));
            this.$emit('submitKnight', newKnight);
            this.$emit('save');
            this.clearKnight();
        }else{
            this.errorForm = true;
        }
    },
    addKnight() {
      this.knight = [];
      this.formatarDate();
      this.knight.push({
        name: this.name,
        nickname: this.nickname,
        birthday: this.newBirthday,
        weapons:[this.weapons],
        attributes: {
          strength: 0,
          dexterity: 0,
          constitution: 0,
          intelligence: 0,
          wisdom: 0,
          charisma: 0
        },
        keyAttribute: "string"
      })
      this.submitForm();
    },
    clearKnight(){
        this.name = null;
        this.nickname = null;
    },
    formatarDate(){
      this.newBirthday = moment(this.birthday, 'yyy/MM/dd').format('YYYY-MM-DDT00:00:00Z');
     }
  },
}
</script>

<template>
  <Transition name="modal">
    <div v-if="show" class="modal-mask">
      <div class="modal-container">
        <div class="modal-header">
          <slot name="header"><h2>Adicionar Knight</h2></slot>
        </div>

        <div class="modal-body">
          <slot name="body">
            <div class="form-row">
                <div class="form-group col-md-6">
                    <label for="nome">Nome</label>
                    <input type="text" v-model="name" placeholder="Nome" class="form-control" id="nome">
                    <div class="error" v-if="errorForm">
                        O nome é obrigatório
                    </div>
                </div>
                <div class="form-group col-md-6">
                    <label for="apelido">Apelido</label>
                    <input type="text" v-model="nickname" placeholder="Apelido" class="form-control" id="apelido">
                </div>
              </div>  
                <div class="form-group">
                    <label for="aniversario">Aniversário</label>
                    <input type="date" v-model="birthday" placeholder="Aniversário" class="form-control" id="aniversario">
                </div>
                <div class="row">
                  <h5>Arma</h5>
                </div>
                <div class="form-row">
                  <div class="form-group col-md-4">
                      <input type="text" v-model="weapons.name" placeholder="Nome da Arma" class="form-control">
                  </div>
                </div>
          </slot>
        </div>

        <div class="modal-footer">
          <slot name="footer">
            <button type="button" class="btn btn-primary" @click="$emit('close')">
                <i class="bi bi-x"></i>
                Cancelar
            </button>
            <button type="button" v-if="salvando == false" class="btn btn-success" @click="addKnight">
                <i class="bi bi-floppy"></i>
                Salvar Knights
            </button>
            <button class="btn btn-primary" v-if="salvando" type="button" disabled>
                <span class="spinner-border spinner-border-sm" role="status" aria-hidden="true"></span>
                Salvando...
            </button>
          </slot>
        </div>

      </div>
    </div>
  </Transition>
</template>

<style>
.modal-mask {
  position: fixed;
  z-index: 9998;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  transition: opacity 0.3s ease;
}

.modal-container {
  width: 700px;
  margin: auto;
  padding: 20px 30px;
  background-color: #fff;
  border-radius: 2px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.33);
  transition: all 0.3s ease;
}

.modal-header h3 {
  margin-top: 0;
  color: #42b983;
}

.modal-body {
  margin: 20px 0;
}

.modal-default-button {
  float: right;
}

.modal-enter-from {
  opacity: 0;
}

.modal-leave-to {
  opacity: 0;
}

.modal-enter-from .modal-container,
.modal-leave-to .modal-container {
  -webkit-transform: scale(1.1);
  transform: scale(1.1);
}
h5{
  margin-left: 20px;
}
.error{
    color:crimson;
}
</style>