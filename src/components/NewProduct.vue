<template> 
 
    <div class="newProduct_user">
        <div class="container_newProduct_user">
            <h2>Ingreso de Mercancía</h2> 
 
            <form v-on:submit.prevent="processNewProduct" >
                
                <label for="name">Nombre del Producto:</label><br>
                <input type="text" v-model="createProduct.name" placeholder="Nombre del Producto" required> 
                <br>
                <label for="username">Nombre de Usuario:</label><br>
                <input type="text" v-model="createProduct.username" placeholder="Nombre de Usuario" readonly> 
                <br>
                <label for="quantity">Cantidad:</label><br>
                <input type="number" v-model="createProduct.quantity" min="1" id="1" placeholder="" required> 
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
                <input type="number" step="any" v-model="createProduct.price" placeholder="" required> 
                <br>

                <button type="submit">Agregar</button><button type="reset">Cancelar</button>

            </form> 
            <br><br><br>
        </div>
    </div> 
</template>

<script>
import gql from "graphql-tag";
 
export default {
    
    name: "NewProduct",
 
    data: function(){ 
        return {
            createProduct: {
                brand: "",
                description: "",
                name: "",
                price: 0,
                quantity: 0,
                type: "",
                username: localStorage.getItem("username")
            }
        }
    },

    methods: {
        processNewProduct: async function() {

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
                alert("Nuevo producto ha sido creado!");
            })
            .catch((error) => {
                alert("Este producto no pudo ser agregado, intenta nuevamente!");
            });
        },
    },
};
</script>

<style>
    .newProduct_user{
        margin: 5%;
        padding: 0%;
        height: 100%;
        width: 100%;
    
        display: flex;
        justify-content: center;
        align-items: center;
    }
    .container_newProduct_user {
        border: 3px solid  #045436;
        border-radius: 10px;
        width: 40%;
        height: 105%;
        
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
    }
    .newProduct_user h2{
        color: #045436;
    }
    .newProduct_user form{
        width: 80%;
        
    }
    .newProduct_user input[type=text]{
        box-sizing: border-box;
        width: 100%;
        padding: 12px 20px;
        margin: 8px 0;
        border: 1px solid  #045436;
    }
    .newProduct_user input[type=number]{
        box-sizing: border-box;
        width: 100%;
        padding: 12px 20px;
        margin: 8px 0;
        border: 1px solid  #045436;
    }
    .newProduct_user button{
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
    .newProduct_user button:hover{
        color: #E5E7E9;
        background: #08A166;
        border: 1px solid #283747;
        cursor: pointer;
    }
</style>