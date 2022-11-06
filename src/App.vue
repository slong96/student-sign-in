<template>

  <div id="app">

    <NewStudentForm v-on:student-added="newStudentAdded"></NewStudentForm>
    <StudentTable v-bind:students="students" 
                  v-on:student-arrived-or-left="studentArrivedOrLeft"
                  v-on:delete-student="studentDeleted"></StudentTable>
    <StudentMessage v-bind:student="mostRecentStudent"></StudentMessage>

  </div>

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
    StudentTable
  },
  data() {
    return {
      students: [],
      mostRecentStudent: {}
    }
  },
  methods: {
    newStudentAdded(student) {
      this.students.push(student)
      this.students.sort(function(s1,s2) {
        return s1.name.toLowerCase() < s2.name.toLowerCase() ? -1 : 1
      })
    },
    studentArrivedOrLeft(student, present) {
      // find student in array of students
      // update present attribute
      let updateStudent = this.students.find(function(s) {
        if (s.name === student.name && s.starID === student.starID) {
          // this is the student to update
          return true
        } 
      })
      if (updateStudent) {
        updateStudent.present = present
        this.mostRecentStudent = updateStudent
      }
    },
    studentDeleted(student) {
      // filter returns a new array of all students for whom the function returns true
      this.students = this.students.filter( function(s) {
        if (s != student) {
          return true
        }
      })
      // clear welcome/goodbye message when deleted
      this.mostRecentStudent = {}
    }
  }
}
</script>

<style>
@import 'https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0/css/bootstrap.min.css';
</style>
