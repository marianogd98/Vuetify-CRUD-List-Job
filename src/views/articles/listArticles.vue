<template>
  <v-container
    id="listArticles"
    fluid
    tag="section"
  >
    <v-btn to="/articles/create" fab dark color="red darken-3"><v-icon>mdi-plus</v-icon></v-btn>

    <base-material-card
      icon="mdi-clipboard-text"
      title="Articulos"
      class="px-5 py-3"
    >
        <v-simple-table
          fixed-header
          class="elevation-3"
        >
            <thead>
                <tr>
                    <th>ID</th>
                    <th>Descripcion</th>
                    <th>Precio</th>
                    <th>Stock</th>
                    <th>Acciones</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="articulo in articulos" :key="articulo.id">
                    <td>{{articulo.id}}</td>
                    <td>{{articulo.descripcion}}</td>
                    <td>{{articulo.precio.toFixed(2)}}</td>
                    <td>{{articulo.stock}}</td>
                    <td>
                        <v-btn :to="{name:'editArticle', params:{id:articulo.id}}" fab small color="light-blue"><v-icon>mdi-pencil</v-icon></v-btn>
                        <v-btn @click.stop="dialog=true" @click="id=articulo.id" fab small color="red darken-3"><v-icon>mdi-delete</v-icon></v-btn>
                    </td>
                </tr>
            </tbody>
        </v-simple-table>
    </base-material-card>
    <v-dialog v-model="dialog" max-width="350">
        <v-card>
            <v-card-title class="headline">Desea eliminar el registro?</v-card-title>
            <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn @click="dialog=false" color="red darken-3">Cancelar</v-btn>
                <v-btn @click="confirmDelete(id)" color="green">Aceptar</v-btn>
            </v-card-actions>
        </v-card>
    </v-dialog>
  </v-container>
</template>
<script>
let url = 'http://localhost:3000/api/articulos/';
import axios from 'axios';
export default {
    name: 'listArticles',
    mounted(){
        this.getArticles();
    },
    data(){
        return{
            dialog:false,
            articulos:null
        }
    }, 
    methods:{
        getArticles(){
            axios.get(url)
            .then(res =>{
                this.articulos = res.data;
            })
            .catch((error)=>{

            })
        },
        confirmDelete(){
            axios.delete(url+this.id)
            .then(()=>{
                this.getArticles();
                this.dialog=false;
            })
            .catch((error)=>{
                console.log(error);
            })
        }
    }
}
</script>

