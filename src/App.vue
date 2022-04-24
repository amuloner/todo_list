<template>
  <div id="app">
    <div class="todo-container">
      <div class="todo-wrap">
        
        <TodoHeader @addTodo="addTodo"></TodoHeader>
        <TodoList :todos="todos"></TodoList>
        <TodoFooter :todos="todos" @checkAllTodo="checkAllTodo" @clearAllTodo="clearAllTodo"></TodoFooter>
      </div>
    </div>
  </div>
</template>

<script>
import TodoHeader from './components/TodoHeader.vue'
import TodoList from './components/TodoList.vue'
import TodoFooter from './components/TodoFooter.vue'


export default {
  name: 'App',
  components: {
    TodoHeader,
    TodoList,
    TodoFooter
  },
  data() {
    return {
      todos:JSON.parse(localStorage.getItem('todos')) || []
    }
  },
  methods:{
    //添加todo
    addTodo(todoObj){
      this.todos.unshift(todoObj);
    },
    //勾选或取消todo
    checkTodo(id){
      this.todos.forEach((todo)=>{
        if(todo.id === id){
          todo.done = !todo.done;
          return;
        }
      })
    },
    //更新todo
    updateTodo(id,title){
      this.todos.forEach((todo)=>{
        if(todo.id === id){
          todo.title = title;
          return;
        }
      })
    },
    //删除todo
    deleteTodo(id){
      this.todos = this.todos.filter((todo)=>{
        return todo.id !== id;
      })
    },
    //全选或全消
    checkAllTodo(done){
      this.todos.forEach((todo)=>{
        todo.done = done;
      })
    },
    //清除所有已经完成的todo
    clearAllTodo(){
      this.todos = this.todos.filter((todo)=> !todo.done);
    }
  },
  watch:{
    todos:{
      handler(value){
        //修改本地存储
        localStorage.setItem('todos',JSON.stringify(value));
      },
      deep:true
    }
  },
  mounted() {
    this.$bus.$on('checkedTodo',this.checkTodo);
    this.$bus.$on('deleteTodo',this.deleteTodo);
    this.$bus.$on('updateTodo',this.updateTodo)
  },
  beforeDestroy(){
    this.$bus.$off(['checkedTodo','deleteTodo'])
  }
}
</script>

<style>
/*base*/
body {
  background: #fff;
}
.btn {
  display: inline-block;
  padding: 4px 12px;
  margin-bottom: 0;
  font-size: 14px;
  line-height: 20px;
  text-align: center;
  vertical-align: middle;
  cursor: pointer;
  box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.2), 0 1px 2px rgba(0, 0, 0, 0.05);
  border-radius: 4px;
}
.btn-danger {
  color: #fff;
  background-color: #da4f49;
  border: 1px solid #bd362f;
}
.btn-edit {
  color: #fff;
  background-color: lightgreen;
  border: 1px solid green;
  margin-right: 5px;
}
.btn-danger:hover {
  color: #fff;
  background-color: #bd362f;
}
.btn-edit:hover {
  color: #fff;
  background-color: green;
}
.btn:focus {
  outline: none;
}
.todo-container {
  width: 600px;
  margin: 0 auto;
}
.todo-container .todo-wrap {
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
}
</style>
