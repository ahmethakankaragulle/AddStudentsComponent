<template>
  <v-app>
    <v-main>
      <AddStudents/>
    </v-main>
  </v-app>
</template>

<script>
import AddStudents from './components/AddStudents.vue';

export default {
  name: 'App',

  components: {
    AddStudents
},

  data: () => ({
    //
  }),
};
</script>
