<template>

  <div class="information">
    <h1>
      <br>
      Hola <span>{{ userDetailById.firstName }}</span
      > <span>{{ userDetailById.lastName }}</span
      >, ¿Qué deseas hacer con el <br> inventario de <span>{{ userDetailById.nameCompany }}</span
      >?
    </h1>

    <hr>
      <button v-on:click="loadNewProduct"> Agregar Productos </button>
      <button v-on:click="loadUpdateProduct"> Actualizar Productos </button>
      <button v-on:click="loadRemoveProduct"> Eliminar Productos </button>
    <hr>

    <div class="details">
      <h3>Perfil del Usuario</h3>
      <h2>
        Nombre: <span>{{ userDetailById.firstName }}</span><br>
        Apellido: <span>{{ userDetailById.lastName }}</span><br>
        Cargo: <span>{{ userDetailById.position }}</span><br>
        Correo electrónico: <span>{{ userDetailById.email }}</span>
      </h2>
    </div>
  </div>
  
</template>


<script>
import gql from "graphql-tag";
import jwt_decode from "jwt-decode";

export default {
    name: "Home",

    data: function(){
        return {
            userId: jwt_decode(localStorage.getItem("token_refresh")).user_id,
            userDetailById: {
                username: "",
                nameCompany: "",
                position: "",
                email: "",
                firstName: "",
                lastName: ""
            }
        }
    },

    methods:{
        loadNewProduct: function() {
            this.$router.push({ name: "newProduct" });
        },
        loadRemoveProduct: function() {
            this.$router.push({ name: "removeProduct" });
        },
        loadUpdateProduct: function() {
            this.$router.push({ name: "updateProduct" });
        }
    },

    apollo: {
      userDetailById: {
        query: gql`
          query ($userId: Int!) {
            userDetailById(userId: $userId) {
              username
              nameCompany
              position
              email
              firstName
              lastName
            }
          }
        `,
        variables() {
          return {
            userId: this.userId,
          };
        },
      },
    }
};

</script>


<style>

.information {
  margin: 0;
  padding: 0%;
  width: 100%;
  height: 100%;

  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.information h1 {
  font-size: 40px;
  text-align: center;
  color: #045436;
}

.information h2 {
  font-size: 30px;
  color: #045436;
}

.information span {
  color: #147287;
  font-weight: bold;
}

hr {
  height: 3px;
  width: 50%;
  background-color: #266E54;
}

.details h3 {
  font-size: 30px;
  color: #045436;
  text-align: center;
}

.details h2 {
  font-size: 24px;
  color: #045436;
}
.details {
  border: 3px solid rgba(0, 0, 0, 0.3);
  border-radius: 10px;
  padding: 0px 80px;
  margin: 10px 0 0 0;
}

.information button{
  width: 25%;
  height: 40px;

  color: #ffffff;
  background: #045436;
  border: 1px solid #ffffff;

  border-radius: 5px;
  padding: 10px 25px;
  margin: 0px 0;
}

.information button:hover{
  color: #ffffff;
  background: #08A166;
  border: 1px solid #045436;
  cursor: pointer;
}

</style>