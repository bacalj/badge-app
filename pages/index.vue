<template>
  <div class="container">

    <div class="password flex items-center justify-center h-screen" v-show="!passwordIn">
      Type in the password: &nbsp;
      <input type="text" v-model="passwordFieldValue" placeholder="type it here">
    </div>

    <div v-show="passwordIn">
      <div v-if="currentStudentData.length > 0">
        <div class="student-name">
          <h2>{{ currentStudentData[0]}}</h2>
        </div>
        <ul class="earnedBadges">
          <!-- really ick at moment, we skip first index because its the name right now, and allOpenBadges var is already built off offset -->
          <li v-for="(badge, i) in currentStudentData" :key="i">
            <div class="badge" v-if="i > 0" :class="{ 'is-earned': badge == 'TRUE', 'not-earned' : badge == 'FALSE' }">
              {{ allOpenBadges[i - 1] }}
            </div>
          </li>
        </ul>
      </div>

      <div v-else>
        Nothing to see here ... you need to go to a student's link.
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

.is-earned {
  color: blue;
}

.not-earned {
  color: silver;
}
</style>>

