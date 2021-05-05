<template>
    <div>
     
        <table class="default" style=" border: #b2b2b2 1px solid;">

  <tr v-for="usuario in usuariosGets.data" :key="usuario.id">

    <td>{{usuario.name}}</td>
    <td>{{usuario.email}}</td> 
    <td>{{usuario.edad}}</td>
    <td>{{usuario.estado}}</td> 
    <td><a v-on:click="eliminarUsuario(usuario.id)">Eliminar</a></td>
    <td><a v-on:click="seleccionarUsuario(usuario)">Actualizar</a></td>
   

  </tr>
 

</table>
<div>
     <form>
<h4>Nombre</h4>
<input type="text" v-model="form.name">
<h4>Correo</h4>
<input type="email" v-model="form.email">
<h4>contrasena</h4>
<input type="text" v-model="form.password">
<h4>Edad</h4>
<input type="number" v-model="form.edad">
<h4>Estado</h4>
<input type="text" v-model="form.estado">

<a v-on:click="crearUsuario()">Guardar</a>
<a v-on:click="actualizarUsuario()">Actualizar</a>
</form>

 </div>
    </div>
</template>

<script>
import axios from 'axios'
export default {
    data(){
        return {
            usuariosGets: null,
            usuarioSeleccionado:null,
            form: {
                name: '',
                email: '',
                password: '',
                edad: '',
                estado: ''
            }
        }
    },
    mounted() {
        this.getTodos();
    },
    methods: {
        getTodos(){
            axios.get('http://localhost:4000/api/user/listUser').then(response => {
                this.usuariosGets = response;
                console.log(this.usuariosGets)

            }).catch(e => console.log(e))
        },
        crearUsuario() {
        // POST request using axios with error handling
        axios.post("http://localhost:4000/api/user/newUser", this.form)
            .then(result=>{ this.getTodos(); console.log(result) })
            .catch(error => {
            this.errorMessage = error.message;
            console.error("There was an error!", error);
            });
        },
        eliminarUsuario(id){
            axios.post("http://localhost:4000/api/user/delete/" + id)
            .then(result=>{ this.getTodos(); console.log(result) })
            .catch(error => {
            this.errorMessage = error.message;
            console.error("There was an error!", error);
            });
        },
        seleccionarUsuario(usuario){
            this.form.id = usuario.id;
            this.form.name = usuario.name;
            this.form.email = usuario.email;
            this.form.password = usuario.password;
            this.form.edad = usuario.edad;
            this.usuarioSeleccionado = usuario;
        },
        actualizarUsuario(){
            var updateUsuario = {
                ide: this.form.id,
                name: this.form.name,
                email:this.form.email,
                edad:this.form.edad,
                password:this.form.password
            }
            console.log(updateUsuario);
            axios.post("http://localhost:4000/api/user/update",updateUsuario)
            .then(result=>{ this.getTodos(); console.log(result) })
            .catch(error => {
            this.errorMessage = error.message;
            console.error("There was an error!", error);
            });
        }
    },
}
</script>

<style>
    
</style>