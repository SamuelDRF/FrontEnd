<template>
  <div class="a" id="app">
    <div class="column">
      <div class="registro">
        <h1><b>Registro</b></h1>
      </div>
      <div class="registro">
          
        <p style="margin-right: 10px"><b>Correio</b></p>
        <input type="email" class="inp" id="user_email" autocomplete="off" placeholder="example@email.com" v-model="email">
        <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-exclamation-circle" viewBox="0 0 16 16" v-if="alerta==1 || alerta==3">
            <path d="M8 15A7 7 0 1 1 8 1a7 7 0 0 1 0 14zm0 1A8 8 0 1 0 8 0a8 8 0 0 0 0 16z"/>
            <path d="M7.002 11a1 1 0 1 1 2 0 1 1 0 0 1-2 0zM7.1 4.995a.905.905 0 1 1 1.8 0l-.35 3.507a.552.552 0 0 1-1.1 0L7.1 4.995z"/>
        </svg>
      </div>

      <div class="registro">
        <p style="margin-right: 10px"><b>Senha</b></p>
        <input type="password" class="inp" id="user_pass" autocomplete="off" placeholder="**********" v-model="password1">
        <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-exclamation-circle" viewBox="0 0 16 16" v-if="alerta==2 || alerta==3 || alerta==4">
            <path d="M8 15A7 7 0 1 1 8 1a7 7 0 0 1 0 14zm0 1A8 8 0 1 0 8 0a8 8 0 0 0 0 16z"/>
            <path d="M7.002 11a1 1 0 1 1 2 0 1 1 0 0 1-2 0zM7.1 4.995a.905.905 0 1 1 1.8 0l-.35 3.507a.552.552 0 0 1-1.1 0L7.1 4.995z"/>
        </svg>
      </div>

      <div class="registro">
        <p style="margin-right: 10px"><b>Senha</b></p>
        <input type="password" class="inp" id="user_pass" autocomplete="off" placeholder="**********" v-model="password2">
        <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-exclamation-circle" viewBox="0 0 16 16" v-if="alerta==2 || alerta==3 || alerta==4">
            <path d="M8 15A7 7 0 1 1 8 1a7 7 0 0 1 0 14zm0 1A8 8 0 1 0 8 0a8 8 0 0 0 0 16z"/>
            <path d="M7.002 11a1 1 0 1 1 2 0 1 1 0 0 1-2 0zM7.1 4.995a.905.905 0 1 1 1.8 0l-.35 3.507a.552.552 0 0 1-1.1 0L7.1 4.995z"/>
        </svg>
      </div>

      <div class="registro">
        <button type="submit" class="button" value="Login" @click="validation()">Criar Conta</button>
      </div>

      <div v-if="alerta==2">
          <p class="alert"> O campo pasword não pode ser nulo</p>
      </div>
      <div v-if="alerta==1">
          <p class="alert">O campo email não pode ser nulo</p>
      </div>
      <div v-if="alerta==3">
          <p class="alert">Os campos email e password não podem ser nulos</p>
      </div>
      <div v-if="alerta==4">
          <p class="alert">As password não são iguais</p>
      </div>

    </div>
  </div>
</template>

<script>
import firebase from "firebase";
export default {
  name: "HomeView",
  data() {
    return{
      email:'',
      password:'',
      password1:'',
      password2:'',
      alerta:0,
    }
  },
  methods:{
    validation(){
      if(this.password1=='' && this.password2=='' && this.email==''){
        this.alerta=3;
      }
      else if(this.email==''){
        this.alerta=1;
      }
      else if(this.password1=='' || this.password2==''){
        this.alerta=2;
      }
      else if(this.password1!='' && this.password2!='' && this.email!=''){
        this.alerta=0;
        this.signupRequest();
      }
      if (this.password1 != this.password2){
          this.alerta=4;
      }
    },

    signupRequest() {
        firebase
        .auth()
        .createUserWithEmailAndPassword(this.email, this.password2)
        .then((userCredential) => {
      // Signed in
      var user = userCredential.user;
      console.log(user);
      // ...
      })
      .catch((error) => {
      var errorCode = error.code;
      var errorMessage = error.message;
      console.log(errorCode);
      console.log(errorMessage);
      // ..
      });
    },
  }
};
</script>

