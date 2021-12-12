<template>

    <div id="Stock">

        <div class="container">
        <h2>Hola <span>{{ username }}</span>!</h2>
        </div>
        <h2>Productos disponibles en esta tienda:</h2>

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
                    <td><button v-on:click="deleteItem(item)">Eliminar</button></td>
                    <td><button v-on:click="updateItem(item)">Editar</button></td>
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
  data: function () {
    return {
      username: localStorage.getItem("username") || "none",
      stockByUsername: [],
    };
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
  },
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
        background-color: crimson;
        color: white;
    }
    #Stock > h2 {
        color: #283747;
        font-size: 25px;
    }
    #Stock .container {
        padding: 30px;
        border: 3px solid rgba(0, 0, 0, 0.3);
        border-radius: 20px;
        margin: 5% 0 1% 0;
    }
    #Stock  .container h2 {
        font-size: 25px;
        color: #283747;
    }
    #Stock .container span {
        color: crimson;
        font-weight: bold;
    }
</style>