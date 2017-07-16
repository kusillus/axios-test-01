<template>
  <div class="checkboxs-container">
    <input type="radio" id="one" value="One" v-model="picked">
    <label for="one">One</label>
    <br>
    <input type="radio" id="two" value="Two" v-model="picked">
    <label for="two">Two</label>
    <br>
  </div>
</template>

<script>
export default {
  props: ["picked"],
  data () {
    return {
    }
  }
}
</script>

<style lang="css">
</style>
