<template>
  <div id="app" class="m-0">
    <cHeader></cHeader>
    <cPrincipal></cPrincipal>
  </div>
</template>

<script>
import cHeader from './components/header.vue'
import cPrincipal from './components/principal.vue'
export default {
  name: 'app',
  data () {
    return {
    }
  },
  components: {
    cHeader,
    cPrincipal
  }
}
</script>
<style lang="scss" src="./scss/index.scss"></style>
