<template>
  <div id ="app">
    <TodoHeader />
    <TodoInput v-on:addTodo="addTodo"
               v-on:editTodo="editTodo"
               v-bind:isEdit="isEdit"
               v-bind:editBefore="editBefore"
               v-bind:itemIndex="itemIndex"
               v-bind:todoItems="todoItems"
    />
    <TodoList
        v-on:removeTodo="removeTodo"
        v-on:editBtn="editBtn"
        v-on:removeAll="clearAll"
        v-on:check="check"
        v-bind:propsdata="todoItems"
        v-bind:message="msg"
    />
  </div>
</template>

<script>
import TodoHeader from './components/TodoHeader.vue'
import TodoList from './components/TodoList.vue'
import TodoInput from './components/TodoInput.vue'

export default {
  name: 'App',
  data: function(){
    return {
      todoItems:[],
      editBefore : "",
      isEdit: false,
      msg:"",
      itemIndex: 0
    }
  },
  created(){
        if(localStorage.getItem('todolist')){
          this.msg =""
          var array = JSON.parse(localStorage.getItem('todolist'))
          for(var i =0; i <array.length; i++){
              this.todoItems.push(array[i].item)
          }
        }
        if(this.todoItems.length === 0){
          this.msg = "오늘은 할일이 없어요."
        }
      },
  methods: {
    addTodo(itemArray, obj){
      if(this.todoItems.indexOf(obj.item) !== -1){
        alert("이미 존재하는 할일 입니다.");
      }
      else{
      this.msg =""
      var array2 = JSON.stringify(itemArray)
      localStorage.setItem('todolist', array2)
      this.todoItems.push(obj.item)
      }
    },
    check(obj2){
      var a =  JSON.stringify(obj2)
      localStorage.setItem('todolist', a)
    },
    editBtn(isEdit, todoItem, index){
      this.isEdit = isEdit
      this.editBefore = todoItem
      this.itemIndex = index
    },
    editTodo(index, editedItem, isEdit2){
      this.isEdit = isEdit2
      this.todoItems.splice(index,1, editedItem)
    },
    removeTodo(todoItem, index){
      localStorage.removeItem(todoItem);
      this.todoItems.splice(index,1);
      if(this.todoItems.length==0){
        this.msg ="할일이 없어요."
      }
    },
    clearAll(){
      localStorage.clear();
      this.todoItems =[];
      this.msg = "할일이 없어요."
    }
  },
  components: {
    TodoHeader,
    TodoList,
    TodoInput
  }
}
</script>

<style lang="scss">
@font-face {
  font-family: 'SANGJUDajungdagam';
  src: url('https://cdn.jsdelivr.net/gh/projectnoonnu/noonfonts_2112@1.0/SANGJUDajungdagam.woff') format('woff');
  font-weight: normal;
  font-style: normal;
}

#app {
  font-family: 'SANGJUDajungdagam';
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
