<template>
  <v-app>


  <div id="re">
  <router-view />
  </div>
  <Navegacion />

    <v-main>

    </v-main>
  </v-app>
</template>

<style>

  #re{
  margin-top:100px;
  }

</style>

<script>
//import HelloWorld from './components/HelloWorld';
import Navegacion from './components/Navegacion.vue';

export default {
  name: 'App',

  components: {
    //HelloWorld,
    Navegacion
  },

  data: () => ({
    //
  }),
};
</script>
