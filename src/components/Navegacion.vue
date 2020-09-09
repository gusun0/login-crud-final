<template>
<v-app>
<v-app-bar
  app
  color="primary"
  dark
>
<v-app-bar-nav-icon @click="drawer = !drawer"></v-app-bar-nav-icon>
  <div class="d-flex align-center">
  LOGIN-CRUD
  </div>


  <v-spacer></v-spacer>

  <v-btn class="success mr-4" :to="{name:'registro'}">
  REGISTARME
  </v-btn>

  <v-btn class="red" :to="{name: 'login'}">
    <span class="mr-2">Iniciar Sesión</span>
  </v-btn>

</v-app-bar>


<v-navigation-drawer app v-model="drawer" temporary dark>
<v-layout mt-4 column align-center>
  <v-flex>
    <v-avatar >
      <img src="https://randomuser.me/api/portraits/men/78.jpg">
    </v-avatar>
  </v-flex>

  <v-flex>
  <p class="mt-3 white--text headline"></p>
  </v-flex>
</v-layout>
<v-layout column>
  <v-flex mx-3>
    <v-btn color="success" block :to="{name: 'home'}">HOME</v-btn>
    <v-btn color="success" block class="my-4" @click="cerrarSesion">Cerrar Sesión</v-btn>
  </v-flex>
</v-layout>

</v-navigation-drawer>




</v-app>

</template>

<style>

  #poin{
  cursor:pointer;
  }

</style>


<script>
import firebase from 'firebase'

export default{

        data(){
        return{
        usuario: null,
        drawer: true

        }
        },

        created(){
        firebase.auth().onAuthStateChanged( usuario => {
            if(usuario){
            this.usuario = usuario
            }
            else{
            this.usuario = null
            }
        })

        },

        methods: {
          cerrarSesion(){

          firebase.auth().signOut()
          .then( () => {
          this.$router.push({name: 'login'})
          })
          }
        }
}
</script>
