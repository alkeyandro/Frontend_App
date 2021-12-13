<template>

  <div id="app" class="app">

    <div class="header">

      <h1> STOCKMASTER </h1>
      <nav>
        <button v-if="is_auth" v-on:click="loadHome"> Inicio </button>
        <button v-if="is_auth" v-on:click="loadStock" > Inventario </button>
        <button v-if="is_auth" v-on:click="logOut"> Cerrar Sesión </button>
        <button v-if="!is_auth" v-on:click="loadLogIn" > Iniciar Sesión </button>
        <button v-if="!is_auth" v-on:click="loadSignUp" > Registrarse </button>
      </nav>
    </div>
    

    <div class="main-component">
      <router-view  
        v-on:completedLogIn="completedLogIn"
        v-on:completedSignUp="completedSignUp"
        v-on:loadUpdateProduct="loadUpdateProduct"
        v-on:loadRemoveProduct="loadRemoveProduct"
        v-on:logOut="logOut"
      >
      </router-view>
    </div>
    

    <div class="footer">
      <h2>
        Misión TIC 2022<br>
        P55 - Grupo 2
      </h2>
    </div>

  </div>

</template>


<script>
export default {
  name: 'App',
  computed: {
    is_auth: {
      get: function() {
        return this.$route.meta.requiresAuth;
      },
      set: function() { }
    }
  },
  methods:{
    loadLogIn: function(){
      this.$router.push({name: "logIn"})
    },
    loadSignUp: function(){
      this.$router.push({name: "signUp"})
    },
    completedLogIn: function(data) {
			localStorage.setItem("username", data.username);
			localStorage.setItem("token_access", data.token_access);
			localStorage.setItem("token_refresh", data.token_refresh);
			alert("Autenticación Exitosa");
			this.loadHome();
    },
    completedSignUp: function(data) {
			alert("Registro Exitoso");
			this.completedLogIn(data);
    },
    loadHome: function() {
      this.$router.push({ name: "home" });
    },
    loadStock: function() {
      this.$router.push({ name: "stock" });
    },
    loadUpdateProduct: function(item, product, amount, detail, clasif, prov, cost) {
      localStorage.setItem("id", item);
      localStorage.setItem("name", product);
      localStorage.setItem("quantity", amount);
      localStorage.setItem("description", detail);
      localStorage.setItem("type", clasif);
      localStorage.setItem("brand", prov);
      localStorage.setItem("price", cost);
      alert("Seleccionó "+product+" con id: "+item);
      this.$router.push({ name: "updateProduct" });
    },
    loadRemoveProduct: function(item) {
      localStorage.setItem("id", item);
      alert("Loading...");
      this.$router.push({ name: "removeProduct" });
    },

    loadProduct: function(item) {
      alert("Loading...");
			this.loadUpdateProduct(item);
    },


    logOut: function () {
			localStorage.clear();
			alert("Sesión Cerrada");
      this.loadLogIn();
		},
  }
}
</script>


<style>

  body{
    margin: 0 0 0 0;
  }

  .header{
    margin: 0%;
    padding: 0;
    width: 100%;
    height: 10vh; 
    min-height: 100px;

    background-color: #045436 ;
    color:#ffffff  ;


    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  .header h1{
    font-family:'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif;
    width: 23%;
    text-align: center;
  }

  .header nav {
    height: 100%;
    width: 30%;

    display: flex;
    justify-content: space-around;
    align-items: center;

    font-size: 20px;
  }

  .header nav button{
    color: #ffffff;
    background: #045436;
    border: 1px solid #08A166;

    border-radius: 5px;
    padding: 10px 20px;
  }

  .header nav button:hover{
    color: #ffffff;
    background: #08A166;
    border: 1px solid #1D543F;
    cursor: pointer;
  }

  
  .main-component{
    height: 70vh;
    margin: 0%;
    padding: 0%;

    background: #ffffff ;
  }

 
  .footer{
    margin: 0;
    padding: 0;
    width: 100%;
    height: 15vh;
    min-height: 100px; 

    background-color: #045436;
    color: #ffffff;

  }

  .footer h2{
    width: 100%;
    height: 100%;
    
    display: flex;
    justify-content: center;
    align-items: center;

  }

</style>