<template>
  <div id="Stock">
    <div class="container">
      <h2>
        ¡Hola <span>{{ username }}</span>!
      </h2>
    </div>
    <h2>Productos disponibles en esta tienda </h2>
    <div class="boton-NewProduct"><button v-on:click="loadNewProduct"> Agrega más productos </button></div><br> 
    
    <div class="container-table">
        <table>
            <thead>
                <tr>
                    <th>Id</th>
                    <th>Nombre</th>
                    <th>Responsable</th>
                    <th>Cantidad</th>
                    <th>Tipo</th>
                    <th>Descripción</th>
                    <th>Marca</th>
                    <th>Precio</th>
                    <th>Eliminar</th>
                    <th>Editar</th>
                </tr>
            </thead>
            <tbody v-for="item in stockByUsername" :key="item.id">
                <tr class="clickable-row">
                    <td>{{ item.id }}</td>
                    <td>{{ item.name }}</td>
                    <td>{{ item.username }}</td>
                    <td>{{ item.quantity }}</td>
                    <td>{{ item.type }}</td>
                    <td>{{ item.description }}</td>
                    <td>{{ item.brand }}</td>
                    <td>$ {{ item.price }}</td>
                    <td><button v-on:click="loadRemoveProduct(item.id)">Eliminar</button></td>
                    <td><button v-on:click="loadUpdateProduct(item)">Editar</button></td>
                </tr>
            </tbody>
        </table>
    </div>
  </div>
</template>

<script>
import gql from "graphql-tag";

export default {
    name: "Stock",

    data: function(){
        return {
            username: localStorage.getItem("username") || "none",
            stockByUsername: []
        }
    },

    methods:{
        loadNewProduct: function() {
            this.$router.push({ name: "newProduct" });
        },

        loadUpdateProduct: function(item) {
            let ref = item.id;
            let product = item.name;
            let amount = item.quantity;
            let detail = item.description;
            let clasif = item.type;
            let prov = item.brand;
            let cost = item.price;
            this.$emit("loadUpdateProduct", ref, product, amount, detail, clasif, prov, cost);
        },

        loadRemoveProduct: function() {
            this.$router.push({ name: "removeProduct" });
        },
    },

    apollo: {
        stockByUsername: {
        query: gql`
            query ($username: String!) {
            stockByUsername(username: $username) {
                id
                name
                username
                quantity
                type
                description
                brand
                price
            }
            }
        `,
        variables() {
            return {
                username: this.username,
            };
            },
    },
  },
  created: function () {
    this.$apollo.queries.stockByUsername.refetch();
  }
};
</script>

<style>
    #Stock {
        width: 100%;
        display: flex;
        justify-content: flex-start;
        align-items: center;
        flex-direction: column;
    }
    #Stock .container-table{
        width:75%;
    
        max-height: 250px;
        overflow-y: scroll;
        overflow-x: hidden;
    }
    #Stock table {
        width: 100%;
        border-collapse: collapse;
        border: 1px solid rgba(0, 0, 0, 0.3);
    }
    #Stock table td,
    #Stock table th {
        border: 1px solid #ddd;
        padding: 8px;
    }
    #Stock table tr:nth-child(even) {
        background-color: #f2f2f2;
    }
    #Stock table tr:hover {
        background-color: #ddd;
    }
    #Stock table th {
        padding-top: 12px;
        padding-bottom: 12px;
        text-align: left;
        background-color: #022116;
        color: white;
    }
    #Stock > h2 {
        color: #555555;
        font-size: 25px;
    }
    #Stock .container {
        padding: 10px 80px;
        border: 3px solid rgba(0, 0, 0, 0.3);
        border-radius: 10px;
        margin: 5% 0 0 0;
    }
    #Stock  .container h2 {
        font-size: 25px;
        color: #045436;
    }
    #Stock .container span {
        color: #147287;
        font-weight: bold;
    }

    button{
        width: 100%;
        height: 40px;

        color: #ffffff;
        background: #045436;
        border: 1px solid #ffffff;

        border-radius: 5px;
        padding: 10px 25px;
        margin: 5px 0;
    }

    button:hover{
        color: #045436;
        background: #08A166;
        border: 1px solid #045436;
        cursor: pointer;
    }
</style>