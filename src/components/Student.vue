<template>
    <div class="container">
        <div class="row justify-content-start">
            <div class="col-12 col-md-6 mb-5">
                <h3 class="text-left">CREAR ESTUDIANTE</h3>
                <div class="card ">
                    <div class="card-body">
                        <form @submit.prevent="save">
                            <div class="form-group">
                                <label class="form-label">Nombre</label>
                                <input type="text" v-model="student.firstName" class="form-control" placeholder="Nombre">
                            </div>
                            <div class="form-group">
                                <label class="form-label">Apellidos</label>
                                <input type="text" v-model="student.lastName" class="form-control" placeholder="Apellido">
                            </div>
                            <div class="form-group mb-3">
                                <label class="form-label">Email</label>
                                <input type="email" v-model="student.email" class="form-control" placeholder="Email">
                            </div>
                            <button type="submit" class="btn btn-primary">Guardar</button>
                        </form>                          
                    </div>
                </div>
            </div>
            <div class="col-12 col-md-12">
                <h3 class="text-left">LISTADO DE ESTUDIANTES</h3>
                <div class="card">
                    <div class="card-body">
                        <table class="table table-dark table-striped">
                            <thead>
                                <tr>
                                <th scope="col">id</th>
                                <th scope="col">Nombre</th>
                                <th scope="col">Apellido</th>
                                <th scope="col">Email</th>
                                <th scope="col">Accion</th>
                                </tr>
                            </thead>
                            <tbody>
                                <tr v-for="student in result" v-bind:key="student.id">
                                    <td>{{ student.id }}</td>
                                    <td>{{ student.firstName }}</td>
                                    <td>{{ student.lastName }}</td>
                                    <td>{{ student.email }}</td>
                                    <td>
                                        <button type="button" class="btn btn-warning" @click="edit(student)">Editar</button>
                                        <button type="button" class="btn btn-danger" @click="remove(student)">Eliminar</button>
                                    </td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>
        </div>        
    </div>
</template>
<script>

import axios from 'axios';

console.log(axios.isCancel('something'));

    export default {
        name: 'Student',
        data (){
            return{
                result:{},
                student:{
                            id:'',
                            firstName: '',
                            lastName: '',
                            email: ''
                }
            }
        },
        created(){
            this.StudentLoad();
        },
        mounted(){
            console.log("mounted() called......")
        },
        methods: {
            StudentLoad(){
                let page = "http://localhost:8000/user"
                axios.get(page)
                .then(
                    ({data})=>{
                        console.log("data",data);
                        this.result = data;
                    }

                )
            },
            save(){
                if(this.student.id == ''){
                    this.saveData();
                }else{
                    this.updateData();
                }
            },
            saveData(){
                console.log("data post", this.student)
                axios.post("http://localhost:8000/user", this.student)
                .then(
                    ({data})=>{
                        console.log("personaje creado");
                        this.StudentLoad();
                        this.student.firstName = '';
                        this.student.lastName = '';
                        this.student.email = '';
                        this.id = ''
                    }
                )
            },
            edit(student){
                this.student = student;
            },
            updateData(){
                var editrecords = 'http://localhost:8000/user/'+ this.student.id; 
                axios.put(editrecords, this.student)
                .then(
                    ({data})=>{
                        this.student.firstName = '';
                        this.student.lastName = '';
                        this.student.email = '';
                        this.id = ''
                        console.log("personaje editado");
                        this.StudentLoad();
                    }
                );
            },
            remove(student){
                console.log("student", student)
                axios.delete(`http://localhost:8000/user/${student.id}`)
                    .then(() => {
                        this.StudentLoad();                        
                        console.log("Personaje eliminado");
                    })
                    .catch(error => {
                        console.error("Error al eliminar el personaje:", error);
                    });                
            }
        }
    }
</script>