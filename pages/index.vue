<template>
  <div class="container">
    hello {{ myroute }}
  </div>
</template>

<script>
import GetSheetDone from 'get-sheet-done';
let DOC_KEY = process.env.DOC_KEY

export default {
  data(){
    return {
      myroute: ''
    }
  },

  mounted(){
    //http://localhost:3000/?foo=%22bar%22 works
    this.myroute = this.$route.query.foo;

    GetSheetDone.raw(DOC_KEY).then(sheet => {
      console.log(sheet)
    })
  }
}
</script>

<style lang="postcss">
.container {
  @apply min-h-screen flex justify-center items-center text-center mx-auto;
}
</style>>

