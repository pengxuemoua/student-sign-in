<template>

  <!-- add all components: NewStudentForm, StudentTable and StudentMessage -->
  <new-student-form v-on:student-added="newStudentsAdded"></new-student-form>
  <student-table v-bind:students="students" 
  v-on:student-arrived-or-left="studentArrivedOrLeft" 
  v-on:delete-student="deleteStudent"></student-table>
  
  <student-message v-bind:student="mostRecentStudent"></student-message>


</template>

<script>
import NewStudentForm from './components/NewStudentForm.vue'; //import components
import StudentMessage from './components/StudentMessage.vue'; //import components
import StudentTable from './components/StudentTable.vue'; //import components

export default {
  name: 'App',
  components: { 
    NewStudentForm, // add components
    StudentMessage,
    StudentTable
  },
  data() {
    return {
      students: [],
      mostRecentStudent: {}
    }
  },
  methods: {
    newStudentsAdded(student) { //create method, from newstudentform object will carry payload used as agrument 
      this.students.push(student)
      this.students.sort(function(s1, s2) {
        return s1.name.toLowerCase() < s2.name.toLowerCase() ? -1 : 1
      })
    },
    studentArrivedOrLeft(student, present) {
      //find student in array of students
      //update present attribute

      let updateStudent = this.students.find( function(s) { 
        if (s.name === student.name && s.starID === student.starID) {
          //this is the student to update
          return true 
        }
      })

      if (updateStudent){
        updateStudent.present = present
        this.mostRecentStudent = updateStudent
      }
    },
    deleteStudent(student) {
      // filter returns a new array of all students for whom the function returns true
      this.students = this.students.filter( function(s) {
        if (s != student) {
          return true
        }
      })
    }

  }
}
</script>

<style>
@import "https://cdn.jsdelivr.net/npm/bootstrap@4.6.0/dist/css/bootstrap.min.css";

</style>
