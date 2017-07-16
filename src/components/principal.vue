<template lang="html">
  <div class="pt-4 container">
    <img class="principal-img" src="../assets/camera.svg">
    <div class="">
      <h3>¿Selecciona tu opcion a buscar?</h3>
      <span>probando axios</span>
    </div>
    <div class="col d-flex justify-content-around">
      <div v-for="item in checkboxs">
          <label class="checkboxs-container" :for="item.value" v-on:click="clear()">{{item.name}}</label>
          <input class="d-none" type="radio" :id="item.value" :value="item.value" v-model="picked">
      </div>
    </div>
    <span>Picked: {{ picked.length }}</span>
    <span>Pokenum: {{ pokeNum.length }}</span>
    <div class="container box-dex mt-4 py-2 px-5 mb-2">
      <h5 class="py-3">Ingresa un numero</h5>
      <input class="form-control" type="text" name="" placeholder="24" v-model="pokeNum">
      <button class="btn" type="button" name="button" v-on:click="submitpk()">Consultar</button>
      <div class="pt-2">
        <span class="poke-sub">Tu pokemon es:</span>
        <p class="poke-res">{{pokeRes}}</p>
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
      emptyNum: false,
      emptyPic: false,
      pokeNum: '',
      pokeRes: '',
      picked: '',
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
              let res = response.data
              vm.pokeRes = res.name
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
