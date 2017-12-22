<template>
  <div class="remuse styleEditor">
    <div class='code' v-html='remuseStyle'></div>
    <pre v-html="remuse"></pre>
  </div>
</template>

<script>
import Prism from 'prismjs'
export default {
  props:['code'],
  computed:{
    remuseStyle:function(){
     return `<style>${this.code}</style>`
   },
   remuse:function(){
    return Prism.highlight(this.code, Prism.languages.css)

   }
  }
}
</script>

<style lang="css">
</style>
