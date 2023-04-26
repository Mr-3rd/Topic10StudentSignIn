<template>

  <new-student-form v-on:student-added="newStudentAdded"></new-student-form>
  <student-table v-bind:students="students"
                 v-on:studentArrivedOrLeft="studentArrivedOrLeft"
                 v-on:delete-student="studentDeleted"
  ></student-table>
  <student-message v-bind:student="latestStudent"
  ></student-message>

</template>

<script>
import NewStudentForm from './components/NewStudentForm.vue'
import StudentMessage from './components/StudentMessage.vue'
import StudentTable from './components/StudentTable.vue'


export default {
  name: 'App',
  components: {
    NewStudentForm,
    StudentMessage,
    StudentTable,
  },
  data() {
    return {
      students: [],
      latestStudent: {}
    }
  },
  methods: {
    newStudentAdded(student) {
      this.students.push(student)
      this.sortStudents();
    },
    sortStudents() {
      // Sor the students array
      this.students.sort(function (s1, s2) {
        return s1.name.toLowerCase() > s2.name.toLowerCase() ? -1: 1
    })
    },
    studentArrivedOrLeft(student, present) {
      // Find student
      // update attribute
      let updateStudent = this.students.find( function (s) {
        if (s.name === student.name && s.starID === student.starID) {
          return true
        }
      })

      if (updateStudent) {
        updateStudent.present = present
        this.latestStudent = updateStudent
      }
    },
    studentDeleted(student) {

      //This seems to be where my code will not delete the rom
      this.students = this.students.filter( function(s) {
        if (s !== student) {
          return true
        }
      })

      //Pressing the delete button will clear the last message
      this.latestStudent = {}
    }
  }
}
</script>

<style>

@import "https://cdn.jsdelivr.net/npm/bootstrap@4.6.0/dist/css/bootstrap.min.css";

</style>
