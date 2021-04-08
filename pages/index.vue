<template>
  <div class="container">

    <div class="password flex items-center justify-center h-screen" v-show="!passwordIn">
      Type in the password: &nbsp;
      <input type="text" v-model="passwordFieldValue" placeholder="type it here">
    </div>

    <div v-show="passwordIn">
      <div v-if="currentStudentData">
        <ul class="earnedBadges">
          <li v-for="(badge, i) in currentStudentData" :key="i">
            <div v-if="i > 0">
              {{ allOpenBadges[i - 1] }} : {{ badge }}
            </div>
          </li>
        </ul>
      </div>

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

</style>>

