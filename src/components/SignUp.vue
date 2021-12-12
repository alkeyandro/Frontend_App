<template>

    <div class="signUp_user">
        <div class="container_signUp_user">
            <h2>Registro</h2>

            <form v-on:submit.prevent="processSignUp" >
                <input type="text" v-model="user.nameCompany" placeholder="Nombre de la Tienda">
                <br>

                <input type="text" v-model="user.position" placeholder="Cargo">
                <br>

                <input type="email" v-model="user.email" placeholder="Correo Electrónico">
                <br>

                <input type="text" v-model="user.username" placeholder="Nombre de Usuario">
                <br>
                
                <input type="text" v-model="user.firstName" placeholder="Nombre">
                <br>

                <input type="text" v-model="user.lastName" placeholder="Apellido">
                <br>

                <input type="password" v-model="user.password" placeholder="Contraseña">
                <br>

                <button type="submit">Crea tú cuenta</button>
            </form>
        </div>

    </div>

</template>


<script>
import gql from "graphql-tag";

export default {
    name: "SignUp",

    data: function() {
        return {
        user: {
            nameCompany: "",
            position: "",
            email: "",
            username: "",
            firstName: "",
            lastName: "",
            password: "",
        },
        };
    },

  methods: {
    processSignUp: async function() {
      await this.$apollo
        .mutate({
          mutation: gql`
            mutation($userInput: SignUpInput!) {
              signUpUser(userInput: $userInput) {
                refresh
                access
              }
            }
          `,
          variables: {
            userInput: this.user,
          },
        })
        .then((result) => {
          let dataLogIn = {
            username: this.user.username,
            token_access: result.data.signUpUser.access,
            token_refresh: result.data.signUpUser.refresh,
          };

          this.$emit("completedSignUp", dataLogIn);
        })
        .catch((error) => {
          alert("ERROR: Fallo en el registro.");
        });
    },

  },
}
</script>


<style>


    .signUp_user{
        margin: 0;
        padding: 0%;
        height: 100%;
        width: 100%;
    
        display: flex;
        justify-content: center;
        align-items: center;
    }

    .container_signUp_user {
        border: 3px solid  #045436;
        border-radius: 10px;
        width: 32%;
        height: 85%;
        
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
    }

    .signUp_user h2{
        color: #045436;

    }

    .signUp_user form{
        width: 70%;
        
    }

    .signUp_user input{
        height: 40px;
        width: 100%;

        box-sizing: border-box;
        padding: 10px 20px;
        margin: 5px 0;

        border: 1px solid #045436;
    }

    .signUp_user button{
        width: 100%;
        height: 40px;

        color: #ffffff;
        background: #045436;
        border: 1px solid #045436;

        border-radius: 5px;
        padding: 10px 25px;
        margin: 5px 0 25px 0;
    }

    .signUp_user button:hover{
        color: #ffffff;
        background: #08A166;
        border: 1px solid #045436;
        cursor: pointer;
    }

</style>