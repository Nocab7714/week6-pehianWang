<template>
  <div>
    <div class="container text-center mt-5">
      <div class="row justify-content-center">
        <h1 class="h3 mb-3 font-weight-normal">請先登入</h1>
        <div class="col-8">
          <form class="form-signin" @submit.prevent="login">
            <div class="form-floating mb-3">
              <input
                type="email"
                class="form-control"
                v-model="user.username"
                id="floatingInput"
                placeholder="name@example.com"
                required
                autofocus
              />
              <label for="floatingInput">Email address</label>
            </div>
            <div class="form-floating">
              <input
                type="password"
                class="form-control"
                v-model="user.password"
                id="floatingPassword"
                placeholder="Password"
                required
              />
              <label for="floatingPassword">Password</label>
            </div>
            <button class="btn btn-lg btn-primary w-100 mt-3" type="submit">登入</button>
          </form>
        </div>
      </div>
      <p class="mt-5 mb-3 text-muted">&copy; 2023~∞ - 六角學院</p>
    </div>
  </div>
</template>

<script>
const { VITE_APP_URL } = import.meta.env

export default {
  data() {
    return {
      user: {
        username: '',
        password: ''
      }
    }
  },
  methods: {
    login() {
      this.$http
        .post(`${VITE_APP_URL}admin/signin`, this.user)
        .then((res) => {
          const { token, expired } = res.data
          // 寫入 cookie token
          // expires 設置有效時間
          document.cookie = `hexToken=${token};expires=${new Date(expired)}; path=/`
          this.$router.push('admin/products')
        })
        .catch((err) => {
          alert(err.res.data.message)
        })
    }
  }
}
</script>

<style>
.form-signin {
  width: 100%;
  max-width: 330px;
  padding: 15px;
  margin: auto;
}
</style>
