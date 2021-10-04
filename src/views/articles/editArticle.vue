<template>
    <v-container>
        <v-row>
            <v-col class="mb-4">
                <h1 class="display-2 font-weight-bold mb-3">Editar de articulos</h1>
            </v-col>
        </v-row>
        <v-row>
            <v-col>
               <form @submit.prevent="saveArticle()">
                    <v-text-field v-model="articulo.descripcion" label="Descripcion" outlined required>
                    </v-text-field>
                    <v-text-field v-model="articulo.precio" label="Precio" type="number" prefix="$" outlined required>
                    </v-text-field>
                    <v-text-field v-model="articulo.stock" label="Stock" type="number" prefix="#" outlined required>
                    </v-text-field>   
                    <v-card-actions>
                        <v-btn type="submit" color="green" class="mr-4">Guardar</v-btn>
                        <v-btn to="/articles" color="red darken-3" class="mr-4">Cancelar</v-btn>
                    </v-card-actions>
                </form> 
            </v-col>
        </v-row>
    </v-container>
</template>
<script>
let url = 'http://localhost:3000/api/articulos/';
import axios from 'axios';
export default {
    name: 'editArticle',
    mounted(){
        this.id=this.$route.params.id;
        axios.get(url+this.id)
        .then(res=>{
            this.articulo=res.data[0];
        })
        .catch((error)=>{
            console.log(error)
        });
    },
    data(){
        return{
            id:null,
            articulo:{
                descripcion:'',
                precio:'',
                stock:''
            }
        }
    },
    methods:{
        saveArticle(){
            let router = this.$router;
            let params = this.articulo;
            axios.put(url+this.id, params)
            .then(()=>{
                router.push('/articles');
            })
            .catch((error)=>{
                console.log(error)
            });
        }
    }
}
</script>