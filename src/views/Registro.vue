<template>

<v-form class="forx"
  ref="form"
  lazy-validation
>
  <v-text-field
    v-model="nombre"
    :rules="nameRules"
    label="Nombre"
    required
  ></v-text-field>

  <v-text-field
    v-model="correo"
    :rules="emailRules"
    label="Correo"
    required
  ></v-text-field>

  <v-text-field
    v-model="pass"
    :rules="passwordRules"
    :type="show1 ? 'text' : 'password'"
    label="Contraseña"
    required
  ></v-text-field>

  <v-btn
    color="primary"
    class="mr-4"
    @click.prevent="agregarUsuario"
  >
  Registrarme
  </v-btn>

</v-form>


</template>


<style>

.forx{
  max-width: 500px;
  margin: 40px auto;
}

</style>


<script>
import {db} from '@/firebase/init.js'
import firebase from 'firebase'


export default{
data(){
return{
  nombre: '',
  correo: '',
  pass: '',
  error: ''
}
},

methods: {
agregarUsuario(){
  if(this.nombre && this.correo && this.pass){
    firebase.auth().createUserWithEmailAndPassword(this.correo, this.pass).then( (u) => {
      this.nombre = ''
      this.correo = ''
      this.pass = ''
      this.$router.push({name: 'crud'})
    }).catch(err => {
      this.error = err.message
    })

  }else{
    this.error = "todos los campos son requeridos"
  }

}
}

}

</script>
