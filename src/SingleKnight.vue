<script>
export default {
    data() {
        return {
            getSingleKnight:{
              id:"",
              name: "",
              nickName: ""
            },
            errorForm: false,
        }
  },
  props: {
    show: Boolean,
    editando: Boolean,
    singleKnight: Object
  },
  methods: {
    editKnight() {
        if(this.getSingleKnight.nickName != null && this.getSingleKnight.nickName != '' ){
            this.errorForm = false;
            const newKnight = JSON.parse(JSON.stringify(this.getSingleKnight));
            this.$emit('editKnightEvent', newKnight);
            this.$emit('save');
            this.clearKnight();
        }else{
            this.errorForm = true;
        }
    },
    clearKnight(){
      this.getSingleKnight = null;
    }
  },

  beforeUpdate() {
    if(this.show){
      this.getSingleKnight = this.singleKnight;
    }
  },
}
</script>

<template>
  <Transition name="modal">
    <div v-if="show" class="modal-mask">
      <div class="modal-container">
        <div class="modal-header">
          <slot name="header"><h2>Editar Knight
          </h2></slot>
        </div>
        <div class="modal-body">
          <slot name="body">
                <div class="form-group">
                    <label for="nome">Nome</label>
                    <input disabled type="text" v-model="getSingleKnight.name" placeholder="Carregando..." class="form-control" id="nome">
                </div>
                <div class="form-group">
                    <label for="apelido">Apelido</label>
                    <input type="text" v-model="getSingleKnight.nickName" placeholder="Apelido" class="form-control" id="apelido">
                </div>
                
          </slot>
        </div>

        <div class="modal-footer">
          <slot name="footer">
            <button type="button" class="btn btn-primary" @click="$emit('close'); clearKnight()">
                <i class="bi bi-x"></i>
                Cancelar
            </button>
            <button type="button" v-if="editando == false" class="btn btn-success" @click="editKnight">
                <i class="bi bi-floppy"></i>
                Editar Knight
            </button>
            <button class="btn btn-primary" v-if="editando" type="button" disabled>
                <span class="spinner-border spinner-border-sm" role="status" aria-hidden="true"></span>
                editando...
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