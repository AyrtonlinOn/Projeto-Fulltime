<template>
  <div class="container d-flex justify-content-center align-items-center my-3">
    <div class="row w-100 justify-content-end">
      <div class="p-5 col-md-5 bg-info rounded-start">
        <div class="fw-bold lh-1 text-white text-start mb-3" style="font-size: 4.5em;">
          Login
          <hr class="border border-white border-3 bg-white">
        </div>
        <h2 class="mb-2 text-white">Usuário</h2>
        <div class="input-group input-group-sm mb-3">
          <input
            v-model="username"
            type="text" 
            class="form-control p-2" 
            aria-label=""
            aria-describedby="inputGroup-sizing-sm"
          >
        </div>
        <h2 class="mb-2 text-white">Senha</h2>
        <div class="input-group input-group-sm mb-3">
          <input 
            v-model="password"
            :type="visibility ? 'text' : 'password'" 
            class="form-control p-2" 
            id="password" 
            aria-label=""
            aria-describedby="inputGroup-sizing-sm"
          >
          <button class="bg-white btn" type="button" id="togglePassword" @click="changeVisibility()">
            <i :class="visibility ? 'bi bi-eye' : 'bi bi-eye-slash'"></i>
          </button>
        </div>
        <div class="d-flex justify-content-between align-items-center mb-3">
          <div class="form-check">
              <span class="text-white">Não possui um login? <a class="text-black" href="/register">Registre-se</a></span>
            </div>
          <button @click="login()" type="button" class="btn btn-light text-dark" style="font-size: 1.5em;">
            Entrar
          </button>
         
        </div>
      </div>
      <div class="col-md-7 bg-white rounded-end d-flex align-items-center justify-content-center">
        <img src="../assets/images/loginAndRegister/login.webp" alt="" class="img-fluid" style="max-width: 80%;">
      </div>
    </div>

    <div :class="'alert alert-' + alertInfo.color" style="position: fixed; bottom: 1em; right: 1em;" role="alert" v-if="alertActivate">
      {{alertInfo.text}}
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data: () => ({
      visibility: false ,
      username: '',
      password: '',
      alertActivate: false,
      alertInfo: {
        color: 'danger',
        text: ''
      },
  }),
  methods: {
      changeVisibility() {
          this.visibility = !this.visibility
      },
      async login() {
        try {
          const response = await axios.get(`https://localhost:7290/api/v1.0/usuarios/${this.username}`);
          
          this.alertInfo.text = 'Login bem-sucedido!';
          this.alertInfo.color = 'success';
          this.alertActivate = true;
          localStorage.setItem('logged', true);
          setTimeout(() => {
            this.$router.replace('/');
            setTimeout(() => {
              location.reload();
            }, 100)
          }, 1000);
        } catch (error) {
          console.error('Erro ao fazer login:', error);
          this.alertInfo.text = 'Erro ao fazer login. Por favor, tente novamente.';
          this.alertInfo.color = 'danger';
          this.alertActivate = true;
        }
      },
  },
  watch: {
    alertActivate(val) {
      if(val) {
        setTimeout(() => {
          this.alertActivate = false
        }, 5000);
      }
    }
  }
};
</script>

<style>

</style>
