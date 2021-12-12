<template>

    <div class="removeProduct_user">
        <div class="container_removeProduct_user">
            <h2>Modificar la Mercancía</h2>

            <form v-on:submit.prevent="processRemoveProduct" >
                
                <label for="username">Nombre de Usuario: </label><br>
                <input type="text" v-model="item.username" placeholder="Nombre de Usuario" required><br>

                <label for="name">Nombre del Producto: </label><br>
                <input type="text" v-model="item.name" placeholder="Nombre del Producto" required><br>

                <label for="brand">Marca: </label><br>
                <input type="text" v-model="item.brand" placeholder="Marca" required><br>
                
                <label for="quantity">Cantidad:</label><br>
                <input type="number" v-model="item.quantity" min="1" id="1" required><br>

                <button type="submit">Eliminar</button>
                <button type="reset">Cancelar</button>

            </form>
        </div>

    </div>

</template>


<script>
import gql from "graphql-tag";

export default {
    name: "RemoveProduct",

    data: function(){
        return {
            username: localStorage.getItem("username") || "none",
            stockByUsername: []
        }
    },

  created: function () {
    this.$apollo.queries.stockByUsername.refetch();
  }
};
</script>


<style>
    .removeProduct_user{
        margin: 0;
        padding: 0%;
        height: 100%;
        width: 100%;
    
        display: flex;
        justify-content: center;
        align-items: center;
    }
    .container_removeProduct_user {
        border: 3px solid  #283747;
        border-radius: 10px;
        width: 30%;
        height: 70%;
        
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
    }
    .removeProduct_user h2{
        color: #283747;
    }
    .removeProduct_user form{
        width: 70%;
        
    }
    .removeProduct_user input[type=text]{
        box-sizing: border-box;
        width: 100%;
        padding: 12px 20px;
        margin: 8px 0;
        border: 1px solid #283747;
    }
    .removeProduct_user input[type=number]{
        box-sizing: border-box;
        width: 100%;
        padding: 12px 20px;
        margin: 8px 0;
        border: 1px solid #283747;
    }
    .removeProduct_user button{
        width: 50%;
        height: 40px;
        color: #E5E7E9;
        background: #283747;
        border: 2px solid #E5E7E9;
        border-radius: 5px;
        padding: 10px 25px;
        margin: 5px 0 25px 0;
        cursor: pointer;
    }
    .removeProduct_user button:hover{
        color: #E5E7E9;
        background: crimson;
        border: 1px solid #283747;
        cursor: pointer;
    }
</style>