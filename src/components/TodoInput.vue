<template>
  <div class="inputSection">
    <div class="addInput">
      <input class="input" type ="text" v-model="newTodoItem" v-on:keyup.enter="addTodo" placeholder="할일을 입력하세요.">
      <span class="btnAdd" v-on:click="addTodo">➕</span>
    </div>
    <div class="editInput">
      <input class="input" v-if="isEdit" v-bind:value="editBefore" v-on:keyup.enter="editTodo" autofocus/>
      <span class="btnEdit" v-if="isEdit" v-on:click="editTodo">✏️</span>
    </div>
  </div>
</template>

<script>
export default {
  props:{
    isEdit:{
      type: Boolean
    },
    editBefore:{
      type: String
    },
    itemIndex:{
      type: Number
    },
    todoItems:{
      type: Array
    }
  },
  data(){
    return {
      newTodoItem: "",
      editItem : this.editBefore,
      itemArray: [],
      isEdit2: this.isEdit,
    }
  },
  methods: {
    addTodo(){
      if(this.newTodoItem ==='') {
          return;
      }
      else{
        let obj = { item: this.newTodoItem && this.newTodoItem.trim(), isCompleted: false}
        this.itemArray.push(obj)
        this.newTodoItem = ""
        this.$emit('addTodo', this.itemArray, obj)
      }
    },
    editTodo(val){
      let editedItem = val.target.value || val.path[1].firstChild.value

      if(this.todoItems.indexOf(editedItem) !== -1){
        alert('이미 존재하는 할일입니다.')
        this.editItem = ""
      }
      else{
        if(editedItem){
          this.editItem = val.target.value
          this.isCompleted = false;

          let obj = { item: editedItem && editedItem.trim(), isCompleted: false}
          let itemArray = JSON.parse(localStorage.getItem('todolist'))
          itemArray[this.itemIndex] = obj
          let itemArray2 = JSON.stringify(itemArray)
          localStorage.setItem('todolist', itemArray2)

          this.editItem=""
          this.isEdit2 = false
          this.$emit('editTodo', this.itemIndex, editedItem, this.isEdit2, itemArray2)
        }
        else{
          alert('수정할 값을 입력해주세요.')
        }
      }
    },
  }
}
</script>

<style scoped>

</style>
