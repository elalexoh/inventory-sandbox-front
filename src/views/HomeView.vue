<script>
import iconLoader from '../components/icons/IconLoader.vue'
import CreateProduct from '../components/core/CreateProduct.vue'
import EditProduct from '../components/core/EditProduct.vue'
export default {
  components: {
    loader: iconLoader,
    createProduct: CreateProduct,
    editProduct: EditProduct
  },
  data() {
    return {
      loading: false,
      store: null,
      products: []
    }
  },
  methods: {
    loadProductsHandler() {
      this.getStore()
    },
    async getStore() {
      this.loading = true
      const res = await fetch('http://127.0.0.1:8000/api/stores')
      const store = await res.json()
      this.store = store.data[0]
      console.debug(this.store)
      this.loading = false
    },
    async removeProduct(id) {
      const options = { method: 'DELETE' }

      fetch(`http://127.0.0.1:8000/api/products/remove/${id}`, options)
        .then((response) => response.json())
        .then((response) => {
          this.getStore()
          console.log(response)
        })
        .catch((err) => console.error(err))
    }
  },
  mounted() {
    this.getStore()
  }
}
</script>


<template>
  <main v-if="!loading">
    <div class="toolbar">
      <createProduct @loadProducts="loadProductsHandler()" :storeData="store" />
    </div>
    <div class="page-wrapper">
      <div class="header">
        <h1 class="store-name">Deeply<span>Store</span></h1>
      </div>
      <div class="content">
        <ul class="product-list" v-if="store">
          <li v-for="product in store.products" :key="product.id" class="product">
            <div class="">
              <h2 class="product__name">
                {{ product.name }}
                <editProduct @loadProducts="loadProductsHandler()" :productData="product" />
                <button class="btn" @click="removeProduct(product.id)">eliminar</button>
              </h2>
              <p class="product__description">{{ product.description }}</p>
            </div>
            <div>
              <p class="product__price">{{ product.price }}$</p>
            </div>
          </li>
        </ul>
      </div>
    </div>
  </main>
  <loader v-else />
</template>

<style lang="scss" >
.page-wrapper {
  min-height: 500px;
  max-width: 750px;
  background-color: #313131;
  padding: 15px 30px;
  border-radius: 10px;

  .header {
    margin-bottom: 30px;
    .store-name {
      text-align: right;
      font-weight: bold;
      font-size: 24px;
      span {
        color: orange;
      }
    }
  }

  .product-list {
    padding: 0 15px;
    list-style: none;
    display: grid;
    grid-template-columns: 1fr;
    gap: 20px;

    .product {
      display: grid;
      grid-template-columns: 1fr 25%;
      align-items: center;
      &__name {
        // font-weight: bold;
        color: orange;
        text-decoration: underline;
        display: flex;
        gap: 10px;
        align-items: center;
        cursor: pointer;

        .btn {
          display: none;
        }
      }
      &__name:hover {
        .btn {
          display: block;
        }
      }
      &__description {
        opacity: 0.5;
      }
      &__price {
        font-size: 24px;
        font-weight: bold;
        text-align: center;
      }
    }
  }
}
.toolbar {
  margin-bottom: 10px;
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