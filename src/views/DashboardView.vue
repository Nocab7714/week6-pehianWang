<template>
  <div>注意!! 這個是"後台頁面"</div>
  <RouterLink to="/admin/products">後台產品列表</RouterLink> |
  <RouterLink to="/admin/orders">後台訂單列表</RouterLink> |
  <RouterLink to="/">返回前台</RouterLink> |
  <a href="" @click.prevent="logout">登出後台</a>
  <hr />
  <router-view></router-view>
</template>

<script>
import { RouterView } from 'vue-router'
const { VITE_APP_URL } = import.meta.env

export default {
  components: {
    RouterView
  },
  methods: {
    logout() {
      document.cookie = `hexToken=; expires=${new Date()};`
      this.$router.push('/login')
      alert('已登出後台系統')
    },
    checkAdmin() {
      //確認登入狀態，並執行相對應行為
      const token = document.cookie.replace(/(?:(?:^|.*;\s*)hexToken\s*=\s*([^;]*).*$)|^.*$/, '$1')
      this.$http.defaults.headers.common.Authorization = token
      this.$http
        .post(`${VITE_APP_URL}/v2/api/user/check`)
        .then((res) => {
          if (!res.data.success) {
            alert(res.data.message)
            this.$router.push('/login')
          } else {
            alert('後台登入成功')
          }
        })
        .catch((err) => {
          console.log(err)
        })
    }
  },
  mounted() {
    this.checkAdmin()
  }
}
</script>
