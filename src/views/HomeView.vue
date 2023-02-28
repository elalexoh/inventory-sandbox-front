<script>
import iconLoader from '../components/icons/IconLoader.vue'
export default {
  components: {
    loader: iconLoader
  },
  data() {
    return {
      loading: false,
      stores: [],
      products: []
    }
  },
  methods: {
    async getStore() {
      this.loading = true
      const res = await fetch('http://inventory-sandbox.test/api/stores')
      const stores = await res.json()
      this.stores = stores.data
      this.loading = false
    },
    async getProducts() {
      this.loading = true
      const res = await fetch('http://inventory-sandbox.test/api/products')
      const products = await res.json()
      this.products = products.data
      this.loading = false
    }
  },
  mounted() {
    this.getStore()
    this.getProducts()
  }
}
</script>


<template>
  <main>
    <div class="toolbar">
      <button class="btn" title="Crear Nuevo producto">Nuevo</button>
    </div>
    <div class="page-wrapper">
      <div class="header">
        <h1 class="store-name">Deeply<span>Store</span></h1>
        <loader />
      </div>
      <div class="content">
        <ul class="product-list">
          <li v-for="product in products" :key="product.id" class="product">
            <div class="">
              <h2 class="product__name">
                {{ product.name }}
                <button class="btn">editar</button>
                <button class="btn">eliminar</button>
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
</template>

<style lang="scss">
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