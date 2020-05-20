<template>
  <div class="columns is-gapless login-container">
    <Head/>
    <div class="column">
      <div class="login-form">
        <Auth title="Login"/>
        <form @submit='login' class="form">
          <div class="form-input">
            <label for="email">Email Address</label>
            <input type="email" id="email" placeholder="rebecca@gmail.com" v-model="email"
            required>
          </div>
          <div class="form-input">
            <label for="password">Password</label>
            <input type="password" id="password" placeholder="*******************"
            v-model='password' required>
          </div>
          <Checkbox/>
          <div class="button-form">
            <button class="button is-black" type="submit">Login</button>
            <router-link to='/register' class="button is-white">Sign Up</router-link>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>

<script>

import axios from 'axios';
import Head from '../components/base/Head.vue';
import Auth from '../components/base/Auth.vue';
import Checkbox from '../components/module/Checkbox.vue';

export default {
  name: 'Login',
  components: {
    Head,
    Auth,
    Checkbox,
  },
  data() {
    return {
      items: [],
      isLogin: true,
      email: null,
      password: null,
      userId: null,
    };
  },
  methods: {
    localData() {
      const parsed = JSON.stringify({
        isLogin: true,
        id: this.userId,
        token: this.token,
        name: this.name,
        role: this.role,
      });
      localStorage.setItem('items', parsed);
      console.log(parsed);
    },
    login(e) {
      e.preventDefault();
      axios
        .post('http://localhost:8000/api/v1/user/login', {
          email: this.email,
          password: this.password,
        }).then((res) => {
          this.userId = res.data.data.id;
          this.token = res.data.data.token;
          this.name = res.data.data.fullname;
          this.role = res.data.data.role_id;
          this.$router.push('/');
          this.$swal.fire({
            icon: 'success',
            html: 'Login Success!',
            showConfirmButton: false,
            timer: 3000,
          });
          this.localData();
        })
        .catch(() => {
          this.$swal.fire({
            icon: 'error',
            title: 'Oops...',
            text: 'Email or Password is Wrong!',
          });
        });
    },
  },
};
</script>

<style scoped>
.form-input {
  border-radius: 5px;
  padding: 10px 20px;
  border: 1px solid #e0e0e0;
}

.form-input input {
  font-family: Airbnb Cereal App;
  border: none;
  display: block;
  font-size: 17px;
  outline: none;
  width: 100%;
  color: #424242;
}

.button {
  border: 1px solid #000;
  font-family: Airbnb Cereal App;
}

.button-form {
  margin-top: 20px;
}

.is-white{
  margin-left: 50px;
}

.is-black:hover{
  transition: 0.5s;
  border: 1px solid #000;
  color: #000;
  background: #ffffff;
}

.is-white:hover{
  transition: 0.5s;
  color: #fff;
  background: #000;
}

.login-form {
  margin-left: 15px;
  margin-right: 50px;
}

@media (max-width: 992px) {
  .login-form {
    transition: 0.5s;
    padding-left: 1em;
    padding-right: 1em;
    width: 100%;
    margin: 0px !important;
  }
}

/* @media (max-width: 576px) {
  .login-form {
    transition: 0.5s;
    padding-left: 1em;
    padding-right: 1em;
    margin: 0px !important;
  }
} */
</style>
