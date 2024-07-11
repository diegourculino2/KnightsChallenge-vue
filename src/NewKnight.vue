<script>
export default {
    data() {
        return {
            knight: [],
            errorForm: false,
            nome: null,
            apelido: null
        }
  },
  props: {
    show: Boolean,
    salvando: Boolean
  },
  methods: {
    submitForm() {
        if(this.knight[0].nome != null && this.knight[0].nome != '' ){
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
      this.knight.push({
        nome: this.nome,
        apelido: this.apelido
      })
      this.submitForm();
    },
    clearKnight(){
        this.nome = null;
        this.apelido = null;
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
                <div class="form-group">
                    <label for="nome">Nome</label>
                    <input type="text" v-model="nome" placeholder="Nome" class="form-control" id="nome">
                    <div class="error" v-if="errorForm">
                        O nome é obrigatório
                    </div>
                </div>
                <div class="form-group">
                    <label for="apelido">Apelido</label>
                    <input type="text" v-model="apelido" placeholder="Apelido" class="form-control" id="apelido">
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
  width: 500px;
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

.error{
    color:crimson;
}
</style>