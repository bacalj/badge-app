<template>
  <div class="container">
    <div class="password" v-show="!passwordIn">
      Type in the password:
      <input type="text" v-model="passwordFieldValue" placeholder="type password">
    </div>

    <div v-show="passwordIn">
      <client-only>
        <div v-for="(badge, i) in badges" :key="i">
          <Badge :texto="badge.title" />
        </div>
      </client-only>
    </div>

  </div>
</template>

<script>
import GetSheetDone from 'get-sheet-done';

let DOC_KEY = process.env.DOC_KEY
let PASSWORD = process.env.PASSWORD

export default {
  data(){
    return {
      studentName: '',
      password: PASSWORD,
      passwordFieldValue: '',
      badges: [
        { title: 'Awesome' },
        { title: 'Cool' }
      ]
    }
  },

  mounted(){
    /* /?student=foobert */
    this.studentName = this.$route.query.student;

    GetSheetDone.raw(DOC_KEY).then(sheet => {
      console.log(sheet)
    })
  },

  computed: {
    passwordIn(){
      return this.passwordFieldValue == this.password
    }
  }
}
</script>

<style lang="postcss">
.container {
  @apply min-h-screen flex justify-center items-center text-center mx-auto;
}
</style>>

