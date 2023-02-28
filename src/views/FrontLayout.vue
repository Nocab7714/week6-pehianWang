<template>
  <div>這是前台頁面</div>
  <RouterLink to="/">Home</RouterLink> | <RouterLink to="/about">About</RouterLink> |
  <RouterLink to="/products">產品列表</RouterLink> | <RouterLink to="/cart">購物車</RouterLink>|
  <RouterLink to="/admin">後台頁面</RouterLink> |
  <RouterLink to="/login">登入後台頁面</RouterLink>
  <hr />
  <router-view></router-view>
</template>
<script>
import { RouterView } from 'vue-router'
export default {
  components: {
    RouterView
  }
}
</script>
