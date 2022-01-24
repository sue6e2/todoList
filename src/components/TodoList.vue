<template>
  <div class="container">
      <ul class="listSection">
        {{ message }}
        <li class="items" v-for="(todoItem, index) in propsdata"
            :key="index"
        >
          <div class="itemSection">
            <input class = "checkbox" type="checkbox" :id ="'checkList_'+index" v-on:change="check(todoItem, index)" />
            <label :for="'checkList_'+index">
              <span v-bind:id="'item_'+index">
                {{ todoItem }}
              </span>
            </label>
            <div class="btnSection">
              <span class="btn" v-on:click="editBtn(todoItem, index)">✏️</span>
              <span class="btn" v-on:click="removeTodo(todoItem, index)">❌</span>
            </div>
          </div>
        </li>
      </ul>
      <button class ="btnClear" v-on:click="clearTodo">Clear All</button>
  </div>
</template>

<script>
export default {
  data(){
    return {
      newTodoItem: "",
      isEdit : false,
      isCompleted: false,
      editItem : "",
    }
  },
  props: {
    propsdata : Array,
    message: String
  },
  methods: {
    check(todoItem, index){
      var isChecked = document.getElementById('checkList_'+index).checked;
      if(isChecked){
        isChecked = false;
        var obj2 = { item : todoItem, isCompleted: true };
        document.getElementById('item_'+ index).style.textDecoration ="line-through";
        document.getElementById('item_'+ index).style.color ="gray";
      }
      else{
        isChecked = true
        obj2 = { item : todoItem, isCompleted: false }
        document.getElementById('item_'+ index).style.textDecoration ="none"
        document.getElementById('item_'+ index).style.color ="black"
      }
      this.$emit('check', obj2, index)
      return isChecked
    },
    editBtn(todoItem, index){
      if(this.isEdit){
        this.isEdit = false
        this.$emit('editBtn', this.isEdit, todoItem, index)
      }
      else{
        this.isEdit = true
        this.editItem = this.propsdata[index]
        this.$emit('editBtn', this.isEdit, todoItem, index)
      }
    },
    removeTodo(todoItem, index){
      this.$emit('removeTodo', todoItem, index)
    },
    clearTodo(){
      this.$emit('removeAll')
    }
  }
}
</script>

<style lang="scss">
.checkbox{
  margin-right: 10px;
}
.inputSection{
  position: relative;
}
.input{
  width: 500px;
  height: 32px;
  font-size: 20px;
  border: 0;
  border-radius: 10px;
  outline: none;
  margin: 10px;
  padding-left: 5px;
  background-color: lightgray;
  font-family: 'SANGJUDajungdagam';
}
.btnAdd, .btnEdit{
  position: relative;
  font-size: 20px;
  right: 50px;
  cursor: pointer;
  align-items: center;
}
.listSection{
  list-style: none;
  width: 500px;
  margin: 0 auto;
  padding: 30px;
}
.items{
  padding: 10px;
  margin: 20px;
  border: solid 1px;
  border-radius: 10px;
  box-shadow: 3px 3px gray;
}
.itemSection{
  font-size: 20px;
  align-items: center;
  display: flex;
}
.btnSection{
  margin-left: auto;
}
.checked{
  text-decoration: line-through;
}
.btn{
  margin: 5px;
  cursor: pointer;
}
.btnClear{
  border-radius: 10px;
  width: 100px;
  height: 50px;
  font-size: 20px;
  border: none;
  cursor: pointer;
  color: red;
  font-family: 'SANGJUDajungdagam';
}
</style>
