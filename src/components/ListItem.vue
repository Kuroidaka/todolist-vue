<template>
    <v-container>
          <v-list v-if="todos">
            <transition-group name="fade">
                <v-list-item v-for="(todo) in todos" :key="todo.id" >
                    <v-list-item-content 
                        v-if="selectItem === todo.id"  
                        class="d-flex align-center pl-10"
                        >
                    <v-text-field 
                        autofocus
                        v-model='currentEditItemValue'  
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
                        transition="fade-transition"
                        v-bind:class="todo.checked? 'active': ''"
                        v-model="todo.checked"
                        :label="todo.title"
                        class="d-flex align-center"
                        />

                        <v-btn 
                            :disabled="todo.checked?true:false"
                            variant="text"
                            @click="handleOpenEditItem(todo.id, $event)" 
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
            </transition-group>
        </v-list>
    </v-container>
</template>

<script>
export default {
    name: 'list-to-do',
    props: ['todos'],
    setup (props) {
        console.log('props', props.todos[0]);
    },
    data: () => ({
        currentEditItemValue: '',
        previousValueOfItem: '',
        selectItem: '',
    }),
    methods: {
        handleOpenEditItem (id, $event) {
            document.querySelectorAll('.v-list-item').forEach((item) => {
                item.classList.add('disable')
            })
            console.log($event.target.closest('.v-list-item').getElementsByTagName('input')[0]);
            $event.target.closest('.v-list-item').classList.remove('disable')
            const currentItem = this.todos.find(item => item.id === id)
            this.previousValueOfItem = currentItem.title
            this.currentEditItemValue = currentItem.title
            this.selectItem = id
        },
        confirmEdited (id) {
            document.querySelectorAll('.v-list-item').forEach((item) => {
                item.classList.remove('disable')
            })
            this.$emit('editItem' ,id, this.currentEditItemValue)
            this.selectItem = ''
            this.currentEditItemValue= ''
            this.previousValueOfItem = ''
        },
        handleInputEditValue ($event) { 
            this.currentEditItemValue = $event.target.value
        },
        closeEdit (id) {
            document.querySelectorAll('.v-list-item').forEach((item) => {
                item.classList.remove('disable')
            })
            if(this.currentEditItemValue !== this.previousValueOfItem) {
                this.$emit('editItem', id, this.previousValueOfItem)
            }
            this.selectItem = ''
            this.previousValueOfItem = ''
            this.currentEditItemValue = ''
        },
        handleRemoveItem (id) {
            this.selectItem = ''
            if(this.selectItem === ''){
                this.$emit('removeItem', id)
            }
        },
        // handleCheck (item) {
        //     const storeItem = item
        //     this.$emit('removeItem', item.id)
        //     this.$nextTick(() => {
        //         this.$emit('addItem', storeItem)
        //     });
            
        //     // this.handleRemoveItem(id)
        // }
    }
}
</script>

<style>

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s;
}
.fade-enter,
.fade-leave-to {
  opacity: 0;
}

.v-list-item.disable {
    opacity: .3;
    pointer-events: none;
}

.v-checkbox.active label {
     text-decoration: line-through
}
</style>