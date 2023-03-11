<template>

   <v-container class="d-flex justify-center align-center flex-column">
      <div class="card-wrapper">
        <v-card>
          <v-card-head class="card-header d-flex justify-space-around">
              <v-card-header-item class="d-flex justify-center align-center">
                <v-icon class="card-header-icon" color='purple'>mdi-view-list</v-icon>
              </v-card-header-item>
          </v-card-head>
          <v-divider></v-divider>
          <v-card-text class="list-wrapper" style="overflow-y: scroll; height: 60vh">
            <ListToDo 
              v-if="todos"
              v-bind:selectItem="selectItem"
              v-bind:todos="todos"
              v-on:removeItem="removeItem" 
              v-on:editItem="editItem"
              v-on:setToggleEditItem="setToggleEditItem"
              v-on:addItem="addItem"
              />
          </v-card-text>
        </v-card>
          <v-btn 
            class="card-btn" 
            icon color='#FFBA74'  
            @click="handleClickAdd">
              <v-icon color='white'>mdi-plus</v-icon>
          </v-btn>
      </div>
      
    </v-container>
</template>

<script>
import { uuid } from 'vue-uuid'
import ListToDo from './ListItem.vue'



export default {
  name: 'ToDoList',
  data: () => ({
      selectItem: -1,
      todos: [ 
            {   
                id:  uuid.v4(),
                title: 'Call the dentist',
                checked: false,
            },
             {   
                id:  uuid.v4(),
                title: 'Call the dentist2',
                checked: false,
            },
             {   
                id:  uuid.v4(),
                title: 'Call the dentist3',
                checked: false,
            },
        ],
  }),
  components: {
    ListToDo: ListToDo,
  },
  methods: {
      removeItem (id) {
        this.todos = this.todos.filter(item => item.id !== id)
      },
      editItem (id, value) { 
        const currentItem = this.todos.find(item => item.id === id)
        currentItem.title = value
      },
      addItem (newItem) {
        this.todos.push(newItem); 
        this.todos = [...this.todos]; 
      },
      handleClickAdd () {
        const newItem = {title: 'New Item', checked: false, id: uuid.v4()}
        this.addItem(newItem)
        this.selectItem = ''
        this.$nextTick(() => {
        var element = document.querySelector('.list-wrapper')
        element.scrollTop = element.scrollHeight;
        });
  }
}
}
</script>

<style scoped>

.modal-wrapper {
  width: 100vw;
  height: 100vh;
}

.card-wrapper {
  width: 500px;
    position: relative;
}

.card-header {
  height: 60px;
  font-size: 20px;
}

.card-header-icon {
  cursor: pointer;
}

.card-btn{
  --btn-size: 60px;
  width: var(--btn-size);
  height: var(--btn-size);
  position: absolute;
  bottom: -30px;
  right: 0;
  left: 0;
  margin-right: auto;
  margin-left: auto;
}

</style>