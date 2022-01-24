<template>
  <div class="inputSection">
    <div class="addInput">
      <input class="input" type ="text" v-model="newTodoItem" v-on:keyup.enter="addTodo" placeholder="할일을 입력하세요.">
      <span class="btnAdd" v-on:click="addTodo">➕</span>
    </div>
    <div class="editInput">
      <input class="input" v-if="isEdit" v-model="editItem" v-on:keyup.enter="editTodo"/>
      {{ editBefore }}
      <span id="editInput" class="btnEdit" v-if="isEdit" v-on:click="editTodo">✏️</span>
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
    }
  },
  data(){
    return {
      todoItems:[],
      newTodoItem: "",
      editItem : "",
      itemArray: [],
      isEdit2: this.isEdit
    }
  },
  watch:{
    isEdit: function (val){
      console.log(val)
    }
  },
  methods: {
    addTodo(){
      // if(this.todoItems.indexOf(this.newTodoItem)==-1){
      //
      // }
      if(this.newTodoItem =='') {
          return
      }
      else{
        var obj = { item: this.newTodoItem && this.newTodoItem.trim(), isCompleted: false};
        this.itemArray.push(obj)
        this.newTodoItem = "";
        this.$emit('addTodo', this.itemArray, obj);
      }
    },
    editTodo(index){
      this.isCompleted = false;
      var editedItem = this.editItem;
      localStorage.setItem(this.editItem, this.isCompleted);
      console.log(index);
      this.isEdit2 = false;
      this.$emit('editTodo', index, editedItem, this.isEdit2);
    },
  }
}
</script>

<style scoped>

</style>
