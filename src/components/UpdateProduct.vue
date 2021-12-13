<template> 
 
    <div class="updateProduct_user">
        <div class="container_updateProduct_user">
            <h2>Modificar la Mercancía | {{ id }}</h2>
 
            <form v-on:submit.prevent="processUpdateProduct" >
                
                <label for="name">Nombre del Producto:</label><br>
                <input type="text" v-model="createProduct.name" placeholder="Nombre del Producto" required> 
                <br>
                <label for="quantity">Cantidad:</label><br>
                <input type="number" v-model.number="createProduct.quantity" id="1" placeholder="" required> 
                <br>
                <label for="type">Tipo o Categoría:</label><br>
                <input type="text" v-model="createProduct.type" placeholder="Tipo" required>
                <br>
                <label for="description">Descripción:</label><br>
                <input type="text" v-model="createProduct.description" placeholder="Descripcion" required>
                <br>
                <label for="brand">Marca:</label><br>
                <input type="text" v-model="createProduct.brand" placeholder="Marca" required> 
                <br>
                <label for="price">Precio:</label><br>
                <input type="number" step="any" v-model.number="createProduct.price" placeholder="" required> 
                <br>

                <button type="submit">Actualizar</button><button type="reset">Cancelar</button>

            </form> 
            <br><br><br>
        </div>
        
    </div> 
</template>

<script>
import gql from "graphql-tag";
 
export default {
    
    name: "UpdateProduct",
 
    data: function(){ 
        return {
            id: localStorage.getItem("id"),
            createProduct: {
                brand: localStorage.getItem("brand"),
                description: localStorage.getItem("description"),
                name: localStorage.getItem("name"),
                price: parseInt(localStorage.getItem("price")),
                quantity: 0,
                type: localStorage.getItem("type"),
                username: localStorage.getItem("username"),
            }
        }
    },

    methods: {
        processUpdateProduct: async function() {

            if (localStorage.getItem("token_access")  === null ||
                localStorage.getItem("token_refresh") === null ) {
                this.$emit("logOut");
                return;
            }
            localStorage.setItem("token_access", "");
            await this.$apollo
                .mutate({
                mutation: gql`
                    mutation ($refresh: String!) {
                    refreshToken(refresh: $refresh) {
                        access
                    }
                    }
                `,
                variables: {
                    refresh: localStorage.getItem("token_refresh"),
                },
                })
                .then((result) => {
                localStorage.setItem("token_access", result.data.refreshToken.access);
                })
                .catch((error) => {
                this.$emit("logOut");
                return;
                });
            
            await this.$apollo
            .mutate({
                mutation: gql`
                    mutation($stock: StockInput!) {
                        createProduct(stock: $stock) {
                            id
                            brand
                            description
                            name
                            price
                            quantity
                            type
                            username
                        }
                    }
                `,
                variables: {
                    stock: this.createProduct,
                },
            })
            .then((result) => {
                alert("Nuevas unidades fueron agregadas!");
            })
            .catch((error) => {
                alert("Error, intenta nuevamente!");
            });
        },
    },
};
</script>

<style>
    .updateProduct_user{
        margin: 5%;
        padding: 0%;
        height: 100%;
        width: 100%;
    
        display: flex;
        justify-content: center;
        align-items: center;
    }
    .container_updateProduct_user {
        border: 3px solid  #045436;
        border-radius: 10px;
        width: 40%;
        height: 103%;
        
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
    }
    .updateProduct_user h2{
        color: #045436;
    }
    .updateProduct_user form{
        width: 70%;
        
    }
    .updateProduct_user input[type=text]{
        box-sizing: border-box;
        width: 100%;
        padding: 12px 20px;
        margin: 8px 0;
        border: 1px solid  #045436;
    }
    .updateProduct_user input[type=number]{
        box-sizing: border-box;
        width: 100%;
        padding: 12px 20px;
        margin: 8px 0;
        border: 1px solid  #045436;
    }
    .updateProduct_user button{
        width: 50%;
        height: 40px;
        color: #E5E7E9;
        background: #045436;
        border: 2px solid #E5E7E9;
        border-radius: 5px;
        padding: 10px 25px;
        margin: 5px 0 25px 0;
        cursor: pointer;
    }
    .updateProduct_user button:hover{
        color: #E5E7E9;
        background: #08A166;
        border: 1px solid #045436;
        cursor: pointer;
    }
</style>