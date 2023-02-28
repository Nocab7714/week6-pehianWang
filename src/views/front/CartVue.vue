<template>
  <table class="table align-middle">
    <thead>
      <tr>
        <th></th>
        <th>品名</th>
        <th style="width: 150px">數量/單位</th>
        <th class="text-end">單價</th>
      </tr>
    </thead>
    <tbody>
      <template v-if="cart.carts">
        <tr v-for="item in cart.carts" :key="item.id">
          <td>
            <button
              type="button"
              class="btn btn-outline-danger btn-sm"
              @click="deleteCartItem(item)"
              :disabled="item.id === loadingItem"
            >
              <!-- <i class="fas fa-spinner fa-pulse"></i> -->
              x
            </button>
          </td>
          <td>
            {{ item.product.title }}
          </td>
          <td>
            <div class="input-group input-group-sm">
              <select
                name=""
                id=""
                class="form-control"
                v-model="item.qty"
                @click="upDataCartItem(item)"
                :disabled="item.id === loadingItem"
              >
                <option v-for="i in 25" :key="i + '123'" :value="i">{{ i }}</option>
              </select>
              <div class="ms-2">
                {{ item.product.unit }}
              </div>
            </div>
          </td>
          <td class="text-end">
            {{ item.total }}
          </td>
        </tr>
      </template>
    </tbody>
    <tfoot>
      <tr>
        <td colspan="3" class="text-end">總計</td>
        <td class="text-end">{{ cart.total }}</td>
      </tr>
      <tr>
        <td colspan="3" class="text-end text-success">折扣價</td>
        <td class="text-end text-success">{{ cart.final_total }}</td>
      </tr>
    </tfoot>
  </table>
</template>

<script>
const { VITE_APP_URL, VITE_APP_PATH } = import.meta.env

export default {
  data() {
    return {
      products: [],
      productId: '',
      cart: {},
      loadingItem: '' // 存 id
    }
  },
  methods: {
    // 取得產品列表
    getProducts() {
      this.$http
        .get(`${VITE_APP_URL}/v2/api/${VITE_APP_PATH}/products/all`)
        .then((res) => {
          this.products = res.data.products
        })
        .catch((err) => {
          alert(err)
        })
    },
    // 打開查看更多 Modal
    openModal(id) {
      this.productId = id
    },
    // 取得購物車資料
    getCart() {
      this.$http
        .get(`${VITE_APP_URL}/v2/api/${VITE_APP_PATH}/cart`)
        .then((res) => {
          this.cart = res.data.data
        })
        .catch((err) => {
          alert(err.message)
        })
    },
    // 更新購物車資料
    upDataCartItem(item) {
      const data = {
        product_id: item.product.id,
        qty: item.qty
      }
      this.$http
        .put(`${VITE_APP_URL}/v2/api/${VITE_APP_PATH}/cart/${item.id}`, { data })
        .then((res) => {
          this.getCart()
        })
        .catch((err) => {
          alert(err.message)
        })
    },
    // 刪除購物車單項項目
    deleteCartItem(item) {
      this.$http
        .delete(`${VITE_APP_URL}/v2/api/${VITE_APP_PATH}/cart/${item.id}`)
        .then((res) => {
          this.getCart()
          alert(res.data.message)
        })
        .catch((err) => {
          alert(err.message)
        })
    },
    // 刪除所有購物車項目
    deleteAllCartItem(item) {
      if (this.cart.length > 0) {
        this.$http
          .delete(`${VITE_APP_URL}/v2/api/${VITE_APP_PATH}/carts`)
          .then((res) => {
            this.getCart()
            alert(res.data.message)
          })
          .catch((err) => {
            alert(err.message)
          })
      } else {
        alert('您的購物車並沒有任何商品喔')
      }
    }
  },
  mounted() {
    this.getProducts()
    this.getCart()
  }
}
</script>
