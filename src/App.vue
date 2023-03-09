<script>
// import { RouterLink, RouterView } from 'vue-router'
import { useVuelidate } from '@vuelidate/core'
import { required, email, minLength } from '@vuelidate/validators'
import { reactive } from 'vue'
import router from "@/router";

export default {
  setup() {
    const state = reactive({
      email: '',
      password: '',
      wasLogin: false
    })
    const rules = {
      email: { required, email },
      password: { required, minLength: minLength(6) },
    }

    const v$ = useVuelidate(rules, state)

    function onChange(e, key) {
      state[key] = e.target.value
      state.wasLogin = false
    }

    function onLogin() {
      state.wasLogin = true
      if(!v$._value.$invalid) {
        router.push('/game')
      }
    }

    return { state, v$, onChange, onLogin }
  },

}
</script>

<template>
  <header>
    <div class="wrapper">
      <nav>
        <RouterLink to="/">Home</RouterLink>
        <RouterLink to="/about">About</RouterLink>
      </nav>
      <div class="main-content">
        <v-card title="Card title" text="">
          <v-text-field @change="(e) => onChange(e, 'email')"  :messages="v$.email.$invalid && state.wasLogin ? 'error' : ''" label="email"/>
          <v-text-field  @change="(e) => onChange(e, 'password')" :messages="v$.password.$invalid && state.wasLogin ? 'error' : ''" label="password"/>
          <v-btn @click="onLogin">
            Sign in
          </v-btn>
        </v-card>
      </div>
    </div>
  </header>

  <RouterView />
</template>

<style scoped>
.wrapper {
  display: flex;
  flex-direction: column;
}
.main-content {
  min-width: 500px;
  background: #ffffff;
  border-radius: 6px;
}
</style>
