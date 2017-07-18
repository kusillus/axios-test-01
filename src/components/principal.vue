<template lang="html">
  <div class="pt-4 container">
    <img class="principal-img" src="../assets/camera.svg">
    <div class="header-principal py-3">
      <h3>¿Selecciona tu opcion a buscar?</h3>
      <span>probando axios</span>
    </div>
    <div class="col d-flex justify-content-around flex-wrap">
      <div class="col-11 col-sm-3  my-1" v-for="item in checkboxs">
        <input class="d-none" type="radio" :id="item.value" :value="item.value" v-model="picked">
        <label class="checkboxs-container py-2 col m-0" :for="item.value" v-on:click="clear()">{{item.name}}</label>
      </div>
    </div>
    <div class="container box-dex mt-1 px-5 mb-2 d-flex flex-wrap justify-content-center">
      <h5 class="py-2 col-12 m-0">Ingresa un numero</h5>
      <input class="form-control col-10" type="text" name="" placeholder="24" v-model="pokeNum">
      <button class="btn col-6 btn-submit my-2" type="button" name="button" v-on:click="submitpk()">Consultar</button>
      <div class="mt-2 col-12 result-query-container">
        <span class="poke-sub">{{message}}</span>
        <div class="d-flex result-query justify-content-center align-items-center">
          <img class="px-1" :src="pokeImg" alt="">
          <p class="poke-res m-0">{{pokeRes}}</p>
        </div>
        <div  v-if="query" class="info-query col-12 d-flex flex-wrap justify-content-center my-3">
          <p class="q-title col-12 py-2">Tu consulta es:</p>
          <p class="q-query col-11">https://pokeapi.co/api/v2/"+<span class="poke">{{picked}}</span>+"/" +<span class="poke">{{pokeNum}}</span></p>
        </div>
      </div>
      <div v-if="emptyNum" class="">
        Debes de ingresar un numero o nombre
      </div>
      <div v-if="emptyPic" class="">
        Debes de seleccionar una categoria
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import _ from 'lodash'
import cCheckbox from './checkboxs.vue'
export default {
  data () {
    return {
      query: false,
      emptyNum: false,
      emptyPic: false,
      pokeNum: '',
      pokeRes: '',
      pokeImg: '',
      picked: '',
      message: '',
      move: 'http://www.pokexperto.net/nds/itemdex/imagenes/329.gif',
      checkboxs: [
        {
          name: "Pokemon",
          value: "pokemon"
        },
        {
          name: "Move",
          value: "move"
        },
        {
          name: "Item",
          value: "item"
        }
      ]
    }
  },
  watch: {
      pokeNum: function() {
      //   this.pokeRes = ''
         if (this.pokeNum.length >= 1 || this.picked.length != 0) {
           this.emptyNum = false
           this.emptyPic = false
         }
      }
  },
  methods: {
    clear: function () {
      this.emptyPic = false
      this.emptyNum = false
      this.pokeNum = "";
    },
    submitpk: function () {
      if (this.pokeNum.length == 0) {
        console.log('nelllll')
        this.emptyNum = true
      }
      else if (this.picked.length == 0) {
        console.log('nelllll')
        this.emptyPic = true
      }
      else {
        this.sendReqPokeNum()
      }
    },
    sendReqPokeNum: _.debounce(function(){
      const vm = this
      vm.pokeRes = 'buscando...'
      axios.get("https://pokeapi.co/api/v2/"+ vm.picked +"/" + vm.pokeNum)
            .then(function(response) {
              vm.query = true
              vm.message = 'tu '+ vm.picked+' es:'
              let res = response.data
              vm.pokeRes = res.name
              if (vm.picked == 'pokemon') {
                vm.pokeImg = res.sprites.front_default
                console.log('pack')
              }
              if(vm.picked == 'item'){
                vm.pokeImg = res.sprites.default
              }
              if (vm.picked == 'move') {
                vm.pokeImg = vm.move
              }
            })
            .catch(function(error) {
              vm.pokeRes = 'Nel'
            })
    },500)
  },
  components: {
    cCheckbox
  }
}
</script>

<style lang="css">
</style>
