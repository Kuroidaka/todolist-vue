<template>
  <v-app>
    <v-main :style="styleObject">
      <ToDoList />
    </v-main>
  </v-app>
</template>

<script>
import ToDoList from './components/ToDoList.vue'

export default {
  data: () => ({
    styleObject: {
      background: 'linear-gradient(#E3ADB7 0%, #AC7EC7 100%)',
    }
  }),
  methods: {
    
  },
  components: {
    ToDoList: ToDoList
  }
};
</script>
