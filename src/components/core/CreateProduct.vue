<template>
  <div class="modal-wrapper" v-if="show">
    <div class="modal">
      <div class="modal__head" v-if="storeData">
        <h2>Crear Producto</h2>
      </div>
      <div class="modal__body">
        <form action="">
          <div class="input-group">
            <label for="product-name">Nombre</label>
            <input type="text" name="product-name" id="product-name" v-model="name" />
          </div>
          <div class="input-group">
            <label for="product-description">Descripción</label>
            <textarea
              name="product-description"
              id="product-description"
              rows="5"
              v-model="description"
            ></textarea>
          </div>
          <div class="input-group">
            <label for="product-price">Precio</label>
            <input type="number" name="product-price" v-model="price" id="product-price" />
          </div>
        </form>
      </div>
      <div class="modal__footer">
        <button class="btn" @click="createProduct()">Aceptar</button>
        <button class="btn" @click="showModalHandler(false)">Cancelar</button>
      </div>
    </div>
  </div>
  <button class="btn" title="Crear Nuevo producto" @click="showModalHandler(true)">Nuevo</button>
</template>

<script>
export default {
  props: ['storeData'],
  emits: ['loadProducts'],
  data() {
    return {
      show: false,
      name: '',
      description: '',
      price: 0
    }
  },
  methods: {
    resetForm() {
      this.name = ''
      this.description = ''
      this.price = 0
    },
    showModalHandler(show) {
      this.show = show
    },
    async createProduct() {
      const options = {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({
          name: this.name,
          description: this.description,
          price: this.price,
          store_id: this.storeData.id
        })
      }

      fetch('http://127.0.0.1:8000/api/products/create', options)
        .then((response) => response.json())
        .then((response) => {
          console.debug(response)
          this.show = false
          this.resetForm()
          this.$emit('loadProducts')
        })
        .catch((err) => console.error(err))
    }
  }
}
</script>

<style lang="scss">
.modal-wrapper {
  position: fixed;
  width: 100%;
  height: 100%;
  background-color: rgba(#313131, 0.75);
  top: 0;
  left: 0;
  z-index: 10;
  display: flex;
  justify-content: center;
  align-items: center;
  grid-template-rows: auto auto auto;
  .modal {
    background-color: #313131;
    border: 2px solid orange;
    padding: 15px 30px 30px 30px;
    border-radius: 15px;
    &__head {
      h2 {
        font-weight: bold;
      }
    }
    &__footer {
      display: flex;
      gap: 10px;
      margin-top: 5px;
      justify-content: flex-end;
    }
    .input-group {
      display: flex;
      flex-direction: column;
      margin-bottom: 15px;
      input,
      textarea {
        border: 0;
        width: 250px;
        height: 30px;
        border-radius: 5px;
        padding: 15px;
      }
      textarea {
        height: unset;
      }
    }
  }
}
.btn {
  // height: 32px;
  // width: 32px;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: orange;
  padding: 5px 10px;
  font-weight: bold;
  color: white;
  font-size: 0.75rem;
  border-radius: 5px;
  transition: all 0.5s ease;
  border: 0;
  &:hover {
    color: orange;
    background-color: white;
  }
}
</style>