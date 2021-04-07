<template>
  <div class="container">
    <client-only>
      <Badge :texto="badgeName" />
    </client-only>
  </div>
</template>

<script>
import GetSheetDone from 'get-sheet-done';
let DOC_KEY = process.env.DOC_KEY

export default {
  data(){
    return {
      myfoo: '',
      badgeName: 'Awesome'
    }
  },

  mounted(){
    //http://localhost:3000/?foo=%22bar%22 works
    this.myfoo = this.$route.query.foo;

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

