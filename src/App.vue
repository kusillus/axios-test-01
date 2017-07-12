<template>
  <div id="app">
    <img class="principal-img" src="./assets/camera.svg">
    <h1>Probando Axios</h1>
    <div class="container box-dex mt-4 py-2 px-5 mb-2">
      <h5 class="py-3">Ingresa un numero</h5>
      <input class="form-control" type="text" name="" placeholder="24" v-model="pokeNum">
      <div class="pt-2">
        <span class="poke-sub">Tu pokemon es:</span>
        <p class="poke-res">{{pokeRes}}</p>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import _ from 'lodash'
export default {
  name: 'app',
  data () {
    return {
      pokeNum: '',
      pokeRes: '',
      msg: 'Welcome to Your Vue.js App'
    }
  },
  watch: {
      pokeNum: function() {
        this.pokeRes = ''
        if (this.pokeNum.length >= 1 && this.pokeNum.length < 4) {
          this.sendReqPokeNum()
        }
      }
  },
  methods: {
    sendReqPokeNum: _.debounce(function(){
      const vm = this
      vm.pokeRes = 'buscando...'
      axios.get("https://pokeapi.co/api/v2/pokemon/" + vm.pokeNum)
            .then(function(response) {
              let res = response.data
              vm.pokeRes = res.name
            })
            .catch(function(error) {
              vm.pokeRes = 'Nel'
            })
    },500)
  }
}
</script>
<style lang="scss" src="./scss/index.scss"></style>
