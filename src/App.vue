<template>
<div class="container">
    <b-dropdown
    text="Seleccionar Articulo"
    variant="secondary"
    >
    <b-dropdown-item
    v-for="(item,index) in groups"
    :key="index"
    @click="mostrar(item.ItmsGrpCod,item.ItmsGrpDesc)"
    >{{item.ItmsGrpNam}}-{{item.ItmsGrpDesc}}</b-dropdown-item>
    </b-dropdown>{{name}}
    <b-dropdown
    text="Seleccionar Color"
    variant="secondary"
    >
    <b-dropdown-item
    v-for="(item,index) in items"
    :key="index"
    @click="mostrar2(item.Color)"
    ><span class="dot" :style="{ backgroundColor: item.ColorHex, borderColor: borderC }"></span>{{item.ColorDesc}}</b-dropdown-item>
    </b-dropdown>
    <table class="table">
  <thead>
    <tr>
      <th scope="col">Estilo</th>
      <th scope="col">Color</th>
      <th scope="col"></th>
      <th scope="col">S</th>
      <th scope="col">M</th>
      <th scope="col">L</th>
      <th scope="col">XL</th>
      <th scope="col">2XL</th>
      <th scope="col">3XL</th>
      <th scope="col">4XL</th>
      <th scope="col">5XL</th>
    </tr>
  </thead>
  <tbody>
    <tr v-for="(item,index) in itemsF" :key="index">
      <th scope="row" >{{item.ItmsGrpNam}}-{{item.ItmsGrpDesc}}</th>
      <td>{{item.ColorDesc}}</td>
      <td><span class="dot" :style="{ backgroundColor: item.ColorHex, borderColor: borderC }"></span></td>
      <td>{{item.Talla}}</td>
      <td v-if="item.Stock>500">+500</td>
      <td v-else>{{item.Stock}}</td>
    </tr>
  </tbody>
</table>
</div>
</template>

<script>
import axios from 'axios'
export default {
  data(){
    return{
      groups:[],
      items:[],
      itemsF:[],
      borderC: 'black',
      code:null,
      name:null
    }
  },
  methods:{
    obtener()
    {
      let my=this;
      axios.get('http://sap.playerytees.com:8091/api/test/ItemGroups')
      .then(function (response) {
        my.groups=response.data.data;
      });
    },
    mostrar(cod,name)
    {
      let my=this;
      axios.get('http://sap.playerytees.com:8091/api/test/Items?ItmsGrpCod='+cod)
      .then(function (response) {
        my.items=response.data.data;
        my.code=cod;
        my.name=name;
      });
    },
    mostrar2(color)
    {
      let my=this;
      axios.get('http://sap.playerytees.com:8091/api/test/Items?ItmsGrpCod='+my.code+'&Color='+color)
      .then(function (response) {
        my.itemsF=response.data.data;
        
      });
    }
  },
  mounted()
  {
    this.obtener();
  }
}
</script>

<style>
.dot {
  height: 25px;
  width: 25px;
  border-radius: 50%;
  display: inline-block;
  border: solid 1px;
}
</style>

