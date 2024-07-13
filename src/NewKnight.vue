<script>
import moment from 'moment';
export default {
    data() {
        return {
            newBirthday: "",
            knight: [],
            errorForm: {
              name: false,
              nickname: false,
              birthday: false,
              weapons: [{
                error: false
              },{
                error: false
              },{
                error: false
              }]
            },
            hasError: false,
            name: "",
            nickname: "",
            birthday: "",
            weapons: [{
              name: "",
              mod: 0,
              attr: "",
              equipped: true
            }],
            attributes: {
              strength: 0,
              dexterity: 0,
              constitution: 0,
              intelligence: 0,
              wisdom: 0,
              charisma: 0
            },
            keyAttribute: "strength",
            numberWeapons: 1
        }
  },
  props: {
    show: Boolean,
    salvando: Boolean
  },
  methods: {
    submitForm() {
        if(this.validateForm()){
            this.hasError = false;
            const newKnight = JSON.parse(JSON.stringify(this.knight[0]));
            this.$emit('submitKnight', newKnight);
            this.$emit('save');
            this.clearKnight();
        }
    },
    addKnight() {
      this.knight = [];
      this.formatarDate();
      this.knight.push({
        name: this.name,
        nickname: this.nickname,
        birthday: this.newBirthday,
        weapons: this.weapons,
        attributes: this.attributes,
        keyAttribute: this.keyAttribute
      })
      this.submitForm();
    },
    clearKnight(){
        this.name = null;
        this.nickname = null;
        this.birthday = null;
        this.weapons = [{
              name: "",
              mod: 0,
              attr: "",
              equipped: true
        }];
        this.attributes = {
              strength: 0,
              dexterity: 0,
              constitution: 0,
              intelligence: 0,
              wisdom: 0,
              charisma: 0
            };
            this.keyAttribute = "strength";
            this.numberWeapons= 1;
    },
    formatarDate(){
      this.newBirthday = moment(this.birthday, 'yyy/MM/dd').format('YYYY-MM-DDT00:00:00Z');
    },
    validateForm(){
      this.hasError = false;
      if(this.knight[0].name == null || this.knight[0].name == ''){
        this.hasError = true;
        this.errorForm.name = true;
        return false;
      }else{this.errorForm.name = false;}

      if(this.knight[0].nickname == null || this.knight[0].nickname == ''){
        this.hasError = true;
        this.errorForm.nickname = true;
        return false;
      }else{this.errorForm.nickname = false;}
      if(this.knight[0].birthday == null || this.knight[0].birthday == '' || this.knight[0].birthday == "Invalid date"){
        this.hasError = true;
        this.errorForm.birthday = true;
        return false;
      }else{this.errorForm.birthday = false;}

     this.weapons.forEach((item, index) => {
        if(this.weapons[index].name == null || this.weapons[index].name == ''){
          this.hasError = true;
          this.errorForm.weapons[index].error = true;
          return false;
        }else{this.errorForm.weapons[index].error = false;}
      })

      return true;
    },
    removeWeapon(){
      if(this.numberWeapons > 1){
        this.weapons.splice(-1);
        this.numberWeapons = this.numberWeapons -1;
      }
    },
    adicionaWeapon(){
      if(this.numberWeapons < 3){
        this.weapons.push({
              name: "",
              mod: 0,
              attr: "",
              equipped: false
        });
        this.numberWeapons = this.numberWeapons +1;
      }
    }
}
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
                <div class="form-group col-md-4">
                    <label for="nome">*Nome</label>
                    <input type="text" v-model="name" placeholder="Nome" class="form-control form-control-sm" id="nome">
                    <div class="error" v-if="hasError && errorForm.name">
                        O nome é obrigatório
                    </div>
                </div>
                <div class="form-group col-md-4">
                    <label for="apelido">*Apelido</label>
                    <input type="text" v-model="nickname" placeholder="Apelido" class="form-control form-control-sm" id="apelido">
                    <div class="error" v-if="hasError && errorForm.nickname">
                        O apelido é obrigatório
                    </div>
                </div>
                <div class="form-group col-md-4">
                    <label for="aniversario">*Nascimento</label>
                    <input type="date" v-model="birthday" placeholder="Aniversário" class="form-control form-control-sm" id="aniversario">
                    <div class="error" v-if="hasError && errorForm.birthday">
                        Data Inválida
                    </div>
                </div>
              </div>
              <div class="card">
                <div class="card-body"> 
                <div class="row">
                  <h5 class="subModal">Atributos</h5>
                </div>
                <div class="form-row">
                  <div class="form-group col-md-2">
                      <label for="strength">Força</label>
                      <input type="number" v-model="attributes.strength" class="form-control form-control-sm" id="strength" min="0" max="20">
                  </div>
                  <div class="form-group col-md-2">
                      <label for="dexterity">Destreza</label>
                      <input type="number" v-model="attributes.dexterity" class="form-control form-control-sm" id="dexterity" min="0" max="20">
                  </div>
                  <div class="form-group col-md-2">
                      <label for="constitution">Constituição</label>
                      <input type="number" v-model="attributes.constitution" class="form-control form-control-sm" id="constitution" min="0" max="20">
                  </div>
                  <div class="form-group col-md-2">
                      <label for="intelligence">Inteligência</label>
                      <input type="number" v-model="attributes.intelligence" class="form-control form-control-sm" id="intelligence" min="0" max="20">
                  </div>
                  <div class="form-group col-md-2">
                      <label for="wisdom">Sabedoria</label>
                      <input type="number" v-model="attributes.wisdom" class="form-control form-control-sm" id="wisdom" min="0" max="20">
                  </div>
                  <div class="form-group col-md-2">
                      <label for="charisma">Carisma</label>
                      <input type="number" v-model="attributes.charisma" class="form-control form-control-sm" id="charisma" min="0" max="20">
                  </div>
                </div>
                <div class="form-group row">
                  <label for="inputState" class="col-sm-3 col-form-label"><b>keyAttribute: </b></label>
                  <div class="col-sm-9">
                    <select v-model="keyAttribute" id="inputState" class="form-control form-control-sm">
                      <option disabled value="">Selecione o atributo para somar ao seu ataque</option>
                      <option value="strength">Força</option>
                      <option value="dexterity">Destreza</option>
                      <option value="constitution">Constituição</option>
                      <option value="intelligence">Inteligência</option>
                      <option value="wisdom">Sabedoria</option>
                      <option value="charisma">Carisma</option>
                    </select>
                  </div>
                </div>
                </div> 
              </div>
                <div class="row">
                  <h5 class="subModal weappon">Armas</h5> 
                  <div class="col">
                    <button type="button" class="btn btn-success btn-sm btn-weapon" @click="adicionaWeapon">
                      <i class="bi bi-plus"></i>
                    </button>
                    <button type="button" class="btn btn-danger btn-sm btn-weapon" @click="removeWeapon">
                      <i class="bi bi-x"></i>
                    </button>
                  </div>
                </div>
                <div class="form-row" v-for="index in numberWeapons" :key="index">
                  <div class="form-group col-sm-5">
                      <input type="text" v-model="weapons[index -1].name" placeholder="Nome da Arma" class="form-control form-control-sm">
                      <div class="error" v-if="hasError && errorForm.weapons[index -1].error">
                        Campo Obrigatório
                    </div>
                  </div>
                  <div class="form-group col-sm-2">
                      <input type="number" v-model="weapons[index -1].mod" class="form-control form-control-sm" id="mod0" min="0" max="20">
                  </div>
                  <div class="form-group col-sm-1"></div>
                  <div class="form-group col-sm-4 d-flex align-items-center">
                    <div class="form-check">
                      <input class="form-check-input" v-model="weapons[index -1].equipped" type="checkbox" :id="'autoSizingCheck' + index">
                      <label class="form-check-label" :for="'autoSizingCheck' + index">
                        Equipada
                      </label>
                    </div>
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

.modal-header{
  padding: 0.5rem !important;
  h2{
    margin-top: 0;
    font-size: 1.3em;
  }
}

.modal-body {
  margin: 0 !important;
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
h5.subModal{
  margin-left: 20px;
  font-size: 1rem !important
}
.error{
    color:crimson;
    font-size:12px;
}
.weappon{
  margin: 15px 0px;
}
.btn-weapon{
  font-size: 14px !important;
  margin-right: 10px !important;
  margin-top: 12px !important;
}
label{
  font-size: 12px !important;
}
</style>