<template>
  <div id="app">
    <NewStudentForm v-on:Student-added="newStudentAdded"></NewStudentForm>
    <!-- Bin students array to table to give to child component -->
    <NewStudentsTable
      v-bind:students="students"
      v-on:student-arrived-or-left="studentArrivedOrLeft"
      v-on:Delete-Student="studentDelete"
    ></NewStudentsTable>

    <NewStudentMessage v-bind:student="mostRecentStudents"></NewStudentMessage>
  </div>
</template>

<script>
import NewStudentForm from './components/NewStudentForm.vue';
import NewStudentMessage from './components/NewStudentMessage.vue';
import NewStudentsTable from './components/NewStudentsTable.vue';

export default {
  name: 'App',
  components: {
    NewStudentForm,
    NewStudentMessage,
    NewStudentsTable,
  },
  data() {
    return {
      students: [],
      mostRecentStudents: {},
    };
  },
  methods: {
    newStudentAdded(student) {
      this.students.push(student);
      this.students.sort(function (s1, s2) {
        return s1.name.toLowerCase() < s2.name.toLowerCase() ? -1 : 1;
      });
    },
    studentArrivedOrLeft(student, present) {
      // Find student in array of students
      // Update present attribute

      // Using find method - this array method takes a function and we create a current index (s) with rule-sets on what to find/match in the array of students, if found found and if any, return true
      let updateStudent = this.students.find(function (s) {
        if (s.name === student.name && s.starID === student.starID) {
          // this student has been found
          return true;
        }
      });

      // If student is found, then update the current present student to new present value
      if (updateStudent) {
        updateStudent.present = present;

        // Give most recent student also updated student
        this.mostRecentStudents = updateStudent;
      }
    },
    studentDelete(student) {
      // Filter returns new array of all students where the function returns true(present)
      this.students = this.students.filter(function (s) {
        // Return all the students besides the given student argument that is being deleted
        if (s != student) {
          return true;
        }
      });

      // Clear most recent student message after deleting
      this.mostRecentStudents = {};
    },
  },
};
</script>

<style>
/* Bootstrap 4 to our app */
@import 'https://cdn.jsdelivr.net/npm/bootstrap@4.6.0/dist/css/bootstrap.min.css';

body {
  background-color: mediumslateblue;
}
</style>
