<template>

    <div class="newProduct_user">
        <div class="container_newProduct_user">
            <h2>Ingreso de Mercancía</h2>

            <form v-on:submit.prevent="processNewProduct" >
             
                <label for="name">Nombre del Producto: </label><br>
                <input type="text" v-model="item.name" placeholder="Nombre del Producto" required><br>

                <label for="username">Nombre de Usuario: </label><br>
                <input type="text" v-model="item.username" placeholder="Nombre de Usuario" required><br>

                <label for="quantity">Cantidad:</label><br>
                <input type="number" v-model="item.quantity" min="1" id="1" required><br>

                <label for="type">Tipo o Categoría: </label><br>
                <input type="text" v-model="item.type" placeholder="Tipo"><br>

                <label for="description">Descripción: </label><br>
                <input type="text" v-model="item.description" placeholder="Descripcion"><br>

                <label for="brand">Marca: </label><br>
                <input type="text" v-model="item.brand" placeholder="Marca" required><br>
                
                <label for="price">Precio:</label><br>
                <input type="number" step="any" v-model="item.price" required><br>

                <button type="submit">Agregar</button>
                <button type="reset">Cancelar</button>

            </form>
        </div>

    </div>

</template>


<script>
    
</script>


<style>
    .newProduct_user{
        margin: 0;
        padding: 0%;
        height: 100%;
        width: 100%;
    
        display: flex;
        justify-content: center;
        align-items: center;
    }

    .container_newProduct_user {
        border: 3px solid  #283747;
        border-radius: 10px;
        width: 30%;
        height: 70%;
        
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
    }

    .newProduct_user h2{
        color: #283747;

    }

    .newProduct_user form{
        width: 70%;
        
    }

    .newProduct_user input[type=text]{
        box-sizing: border-box;
        width: 100%;
        padding: 12px 20px;
        margin: 8px 0;
        border: 1px solid #283747;
    }

    .newProduct_user input[type=number]{
        box-sizing: border-box;
        width: 100%;
        padding: 12px 20px;
        margin: 8px 0;
        border: 1px solid #283747;
    }

    .newProduct_user button{
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

    .newProduct_user button:hover{
        color: #E5E7E9;
        background: crimson;
        border: 1px solid #283747;
        cursor: pointer;
    }
</style>