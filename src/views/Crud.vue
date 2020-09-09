<template>
<div>

<v-form class="forx"
  ref="form"
  lazy-validation

  v-if="agregaFru"
>
  <v-text-field
    v-model="nombre"
    label="nombre de fruta"
    required
  ></v-text-field>

  <v-text-field
    v-model="cantidad"
    :type="number"
    label="cantida de frutas"
    required
  ></v-text-field>

  <v-btn
    color="primary"
    class="mr-4"
    @click.prevent="guardarFruta"
  >
  AGREGAR
  </v-btn>

</v-form>

<v-form
  ref="form"
  lazy-validation
  v-if="!agregaFru"
>
  <v-text-field
    v-model="nombre"
    label="nombre de fruta"
    required
  ></v-text-field>

  <v-text-field
    v-model="cantidad"
    :type="number"
    label="cantida de frutas"
    required
  ></v-text-field>

  <v-btn
    color="primary"
    class="mr-4"
    @click.prevent="editarT"
  >
  EDITAR
  </v-btn>

</v-form>


<br><br>


<div class="tabla">
<v-simple-table border="1" >
  <template v-slot:default>
    <thead>
      <tr>
      </tr>
    </thead>
    <tbody>
      <tr v-for="(item,index) in frutas" :key="index">
      <td>ID: {{item.id}}</td>
      <td>FRUTA: {{item.nf}}</td>
      <td>CANTIDAD: {{item.cf}}</td>
      <td>
      <v-btn color="warning" @click="editar(index,item.id)">
      <span class="material-icons">
        edit
        </span>
      </v-btn>
      <v-btn color="red" class="ml-2"@click="eliminar(index,item.id)">
      <span class="material-icons">
delete
</span  >
      </v-btn>
      </td>
      </tr>
    </tbody>
  </template>
</v-simple-table>
</div>


</div>
</template>

<style>

div .tabla{
margin-left: 200px;
}

.forx{
max-width: 500px;
margin: 40px auto;
}

#a{
max-width: 500px;
margin: 40px auto;
}

</style>



<script>
import {db} from '@/firebase/init.js'
//import {coleccion} from '@/firebase/init.js'
//import firebase from 'firebase'

export default{
  data(){
    return{
      nombre: null,
      cantidad: null,
      id: null,
      index: null,
      frutas: [

      ],
      error: '',
      agregaFru: true
    }
  },

  methods:{

  created(){
    this.listarFrutas();
  },


  async listarFrutas(){
    try{

      const reDB = await db.collection('frutas').get()

      reDB.forEach(res => {
        console.log(res.id);
        const fru = {
        id: res.id,
        nf: res.data().nombre,
        cf: res.data().cantidad
        }
        this.frutas.push(fru)

      })

    }catch(error){
      console.log(error)


    }
  },


  async guardarFruta(){
  try{

    const reDB = await db.collection('frutas').add({
    nf: this.nombre,
    cf: this.cantidad
    })

    this.frutas.push({
    nf: this.nombre,
    cf: this.cantidad,
    id: reDB.id
    })
    this.nombre = ''
    this.cantidad = ''

    } catch(error){
    console.log(error)
  }





  },
  async eliminar(index,id){
  try{

    await db.collection('frutas').doc(id).delete()
    this.frutas.splice(index,1)


  }catch(error){
    console.log(error)
  }

  },

  editarT(index){
  this.agregaFru = false
  this.nombre = this.frutas[index].nf
  this.cantidad = this.frutas[index].cf
  },

  async editar(index,id){

  try{

    await db.collection('frutas').doc(id).update({
    nf: this.nombre,
    cf: this.cantidad
    })
    this.frutas[index].nf = this.nombre
    this.frutas[index].cf = this.cantidad

  }catch(error){
  console.log(error)
  }
  }



  }


  /*

  mounted(){
  this.frutas=[]
  coleccion.get()
  .then( (r) => r.docs.map( (item) => {
      this.frutas.push({id:item.id,data:item.data()})
  })).catch(err => {
    this.error="error"
  })

  },


  methods:{

,

  agregarFruta(){
  coleccion.add({nombre: this.nombre, cantidad: this.cantidad})
  .then( () => this.$mount())
  },

  editarFruta(id){
  coleccion.doc(id).set({nombre:this.nombre,cantidad:this.cantidad})
  .then( () => this.$mount())
  },

  eliminarFruta(id){
  coleccion.doc(id).delete()
  .then( () => this.$mount())
  }
  }
*/
}

</script>
