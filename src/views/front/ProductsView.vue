<template>
  <div>這是產品列表</div>
  <table class="table">
    <tbody>
      <tr v-for="product in products" :key="product.id">
        <td>{{ product.title }}</td>
        <td><img :src="product.imageUrl" class="img-fluid" width="200" alt="" /></td>
        <td>
          <router-link :to="`/product/${product.id}`" class="btn btn-outline-secondary me-3"
            >產品細節</router-link
          >
          <button type="button" class="btn btn-primary" @click="addToCart(product.id)">
            加入購物車
          </button>
        </td>
      </tr>
    </tbody>
  </table>
</template>

<script>
import { RouterLink } from 'vue-router'
const { VITE_APP_URL, VITE_APP_PATH } = import.meta.env

export default {
  data() {
    return {
      products: []
    }
  },
  components: {
    RouterLink
  },
  methods: {
    getProducts() {
      this.$http.get(`${VITE_APP_URL}/v2/api/${VITE_APP_PATH}/products/all`).then((res) => {
        console.log(res)
        this.products = res.data.products
      })
    },
    addToCart(id) {
      const data = {
        product_id: id,
        qty: 1
      }
      this.$http.post(`${VITE_APP_URL}/v2/api/${VITE_APP_PATH}/cart`, { data }).then((res) => {
        alert(res.data.message)
      })
    }
  },
  mounted() {
    this.getProducts()
  }
}
</script>
