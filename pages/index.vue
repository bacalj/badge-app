<template>
  <div class="container">
    <h2 v-if="currentStudentData">Student: {{ currentStudentData[0] }}</h2>

    <div class="password" v-show="!passwordIn">
      Type in the password:
      <input type="text" v-model="passwordFieldValue" placeholder="type password">
    </div>

    <div v-show="passwordIn">
      ok u are in
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
      studentName: null,
      password: PASSWORD,
      passwordFieldValue: '',
      allOpenBadges: [],
      currentStudentData: []
    }
  },

  mounted(){
    /* /?student=foobert */
    if (this.$route.query.student){
      this.studentName = this.$route.query.student.toLowerCase();
    }


    GetSheetDone.raw(DOC_KEY).then(sheet => {

      // get a list of all the badges
      this.allOpenBadges = sheet.data[0].slice(1);

      // if we have a query param for a student, pull their data into localness
      if ( this.studentName ){
        sheet.data.forEach(arr => {
          let myhead = arr[0].toLowerCase();
          if (myhead == this.studentName){
            this.currentStudentData = arr;
          }
        });
      }
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
  width:300px;
}
</style>>

