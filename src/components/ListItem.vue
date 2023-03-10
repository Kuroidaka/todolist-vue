<template>
    <v-container>
          <v-list>
              <v-list-item v-for="(todo) in todos" :key="todo.id">
                <v-list-item-content 
                    v-if="selectItem === todo.id"  
                    class="d-flex align-center pl-10"
                    >
                   <v-text-field 
                    v-model='todo.title'  
                    variant="underlined"
                    @input="handleInputEditValue($event)"
                     />

                    <v-btn icon variant="text" @click="confirmEdited(todo.id)">
                        <v-icon color="green">mdi-check</v-icon>
                    </v-btn>
                    <v-btn icon variant="text" @click="closeEdit(todo.id)">
                        <v-icon color="red">mdi-close</v-icon>
                    </v-btn>
                </v-list-item-content>
                
                <v-list-item-content v-else class="d-flex align-center"> 
                   <v-checkbox 
                    v-bind:class="todo.checked? 'active': ''"
                    v-model="todo.checked"
                    :label="todo.title"
                     class="d-flex align-center"
                    />

                    <v-btn 
                        variant="text"
                        @click="handleOpenEditItem(todo.id)" 
                        style="cursor: pointer; user-selector: none"
                        class="ml-4 text-button">EDIT</v-btn>
                    <v-btn 
                        variant="text"
                        class="text-button"
                        style="color: #e90000; cursor: pointer"
                        @click="handleRemoveItem(todo.id)" 
                        >DELETE</v-btn>
                </v-list-item-content>
              </v-list-item>
            </v-list>
    </v-container>
</template>

<script>
export default {
    name: 'list-to-do',
    props: {
        todos: {
            type: Array,
        },
    },
    data: () => ({
        currentEditItemValue: '',
        previousValueOfItem: '',
        selectItem: -1
    }),
    methods: {
        handleOpenEditItem (id) {
            this.previousValueOfItem = this.todos[id].title
            this.currentEditItemValue = this.todos[id].title
            this.selectItem = id
        },
        confirmEdited (id) {
            this.$emit('editItem' ,id, this.currentEditItemValue)
            this.selectItem = -1
            this.currentEditItemValue= ''
            this.previousValueOfItem = ''
        },
        handleInputEditValue ($event) { 
            this.currentEditItemValue = $event.target.value
        },
        closeEdit (id) {
            if(this.currentEditItemValue !== this.previousValueOfItem) {
                this.$emit('editItem', id, this.previousValueOfItem)
            }
            this.selectItem = -1
            this.previousValueOfItem = ''
            this.currentEditItemValue = ''
        },
        handleRemoveItem (id) {
            this.selectItem = -1
            if(this.selectItem === -1){
                this.$emit('removeItem', id)
            }
        }
    }
}
</script>

<style>
.v-checkbox.active label {
     text-decoration: line-through
}
</style>