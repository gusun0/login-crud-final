<template>

<v-form class="forx"
  ref="form"
  lazy-validation
>
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
    @click.prevent="iniciarSesion"
  >
  Iniciar Sesión
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
  correo: '',
  pass: '',
  error: ''
}
},

  methods:{
  iniciarSesion(){
  this.error = ''
  if(this.correo && this.pass){
    firebase.auth().signInWithEmailAndPassword(this.correo,this.pass)
    .then ( (u) => {
      this.$router.push({name: 'crud'})
      }).catch( err => {
        this.error = err.message
      })

  }else{
    this.error = "todos los campos son requeridos"
  }

  }

  }

}

</script>
