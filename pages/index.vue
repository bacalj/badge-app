<template>
  <div class="container">
    hello
  </div>
</template>

<script>
import GetSheetDone from 'get-sheet-done';
let DOC_KEY = process.env.DOC_KEY

export default {
  mounted(){
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

