<template>
  <div class="">
    <nav class="navbar navbar-expand-lg navbar-light bg-light">
      <a class="navbar-brand" href="#">Desafio Capgemini</a>
      <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
      </button>

      <div class="collapse navbar-collapse" id="navbarSupportedContent">
        <ul class="navbar-nav mr-auto">
          <li class="nav-item active">
            <router-link class="nav-link" v-if="!hash || hash.length === 0" to="/">Home</router-link>
          </li>
          <li class="nav-item active">
            <router-link class="nav-link" v-if="hash && hash.length != 0" to="/dashboard">Dashboard</router-link>
          </li>
          <li class="nav-item">
            <router-link class="nav-link" to="/about">About</router-link>
          </li>
        </ul>
        <form class="form-inline my-2 my-lg-0" v-if="!hash || hash.length === 0">
          <router-link class="nav-link" to="/login">Login</router-link>
          <router-link class="btn btn-outline-primary my-2 my-sm-0" to="/register">Register</router-link>
        </form>
        <form @submit.prevent="logout" class="form-inline my-2 my-lg-0" v-if="hash && hash.length != 0">
          <button type="submit" class="btn btn-outline-primary my-2 my-sm-0">Logout</button>
        </form>
      </div>
    </nav>
    <br/>
    <router-view />
  </div>
</template>

<script>
  import axios from "axios";

  export default {
      name: "login",
      data() {
          return {
              hash: localStorage.getItem('client_hash')
          }
      },
      methods : {
          checkLogin() {
            let data = new FormData();
            data.append('hash', localStorage.getItem('client_hash'));
            axios.post(process.env.VUE_APP_API_URL + '/api/check-login', data).then((res) => {
                console.log(res);
                this.$router.push('dashboard');
            }).catch((error) => {
              console.log(error);
            });
          },
          logout() {
            localStorage.removeItem('client_hash');
            window.location.href = '/';
            // this.$router.push('/');
          }
      },
      mounted() {
          this.checkLogin();
      }
  }
</script>