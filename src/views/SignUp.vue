<template>
  <div class="container py-5">
    <form class="w-100" @submit.prevent.stop="handleSubmit">
      <div class="text-center mb-4">
        <h1 class="h3 mb-3 font-weight-normal">
          Sign Up
        </h1>
      </div>

      <div class="form-label-group mb-2">
        <label for="name">Name</label>
        <input
          id="name"
          name="name"
          type="text"
          v-model="name"
          class="form-control"
          placeholder="name"
          autocomplete="username"
          required
          autofocus
        >
      </div>

      <div class="form-label-group mb-2">
        <label for="email">Email</label>
        <input
          id="email"
          name="email"
          type="email"
          v-model="email"
          class="form-control"
          placeholder="email"
          autocomplete="email"
          required
        >
      </div>

      <div class="form-label-group mb-3">
        <label for="password">Password</label>
        <input
          id="password"
          name="password"
          type="password"
          v-model="password"
          class="form-control"
          placeholder="Password"
          autocomplete="new-password"
          required
        >
      </div>

      <div class="form-label-group mb-3">
        <label for="password-check">Password Check</label>
        <input
          id="password-check"
          name="passwordCheck"
          type="password"
          v-model="passwordCheck"
          class="form-control"
          placeholder="Password"
          autocomplete="new-password"
          required
        >
      </div>

      <button
        class="btn btn-lg btn-primary btn-block mb-3"
        type="submit"
      >
        Submit
      </button>

      <div class="text-center mb-3">
        <p>
          <router-link to="/signin">
            Sign In
          </router-link>
        </p>
      </div>

      <p class="mt-5 mb-3 text-muted text-center">
        &copy; 2017-2018
      </p>
    </form>
  </div>
</template>

<script>
import authorizationAPI from './../apis/authorization'
import { Toast } from './../utils/helpers'

export default {
    data () {
        return {
            name: '',
            email: '',
            password: '',
            passwordCheck: ''

        }
    },
    methods: {
        async handleSubmit () {
          try{
            if (!this.email || !this.password || !this.name || !this.passwordCheck) {
              Toast.fire({
                icon: 'warning',
                title: '請填入 name、email 和 password'
              })
              return
            }
            if ( this.password !== this.passwordCheck ) {
              Toast.fire({
                icon: 'warning',
                title: '請輸入相同的密碼'
              })
              return
            }

            const { data } = await authorizationAPI.signUp({
              name: this.name,
              email: this.email,
              password: this.password,
              passwordCheck: this.passwordCheck
            })

            if (data.status !== 'success') {
              throw new Error(data.message)
            }
            Toast.fire({
              icon: 'success',
              title: '加入成功！'
            })

            this.$router.push('/signin')
          }
          catch (error) {
            console.log('error', error)
            Toast.fire({
              icon: 'error',
              title: '無法加入會員，請稍後再試'
            })
          }
      
    }
  }
}
</script>