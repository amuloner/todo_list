<template>
  <transition name="todo" appear>
    <li>
    <label>
      <input type="checkbox" :checked="todoObj.done" @change="handleCheck(todoObj.id)"/>
      <span>{{todoObj.title}}</span>
    </label>
    <button class="btn btn-danger" @click="handleDelete(todoObj.id)">删除</button>
    <button class="btn btn-edit" @click="handleEdit(todoObj)" v-show="!todoObj.isEdit">编辑</button>
  </li>
  </transition>
</template>

<script>
    export default {
        name: 'TodoItem',
        props:['todoObj','checkTodo','deleteTodo'],
        methods: {
          handleCheck(id){
            //通知app将对应done值取反
            this.checkTodo(id);
          },
          handleDelete(id){
            if(confirm("确定删除吗？")){
              this.deleteTodo(id);
            }
          },
          handleEdit(todoObj){
            console.log(todoObj);
          }
        },
    }
</script>

<style scoped>
/*item*/
li {
  list-style: none;
  height: 36px;
  line-height: 36px;
  padding: 0 5px;
  border-bottom: 1px solid #ddd;
}
li label {
  float: left;
  cursor: pointer;
}
li label li input {
  vertical-align: middle;
  margin-right: 6px;
  position: relative;
  top: -1px;
}
li button {
  float: right;
  display: none;
  margin-top: 3px;
}
li:before {
  content: initial;
}
li:last-child {
  border-bottom: none;
}
li:hover{
  background-color: #ddd;
}
li:hover button{
  display: block;
}
.todo-enter-active{
  animation: todo 0.5s linear;
}
.todo-leave-active{
  animation: todo 0.5s linear reverse;
}
@keyframes todo {
  from{
    transform: translateX(100%);
  }
  to{
    transform: translateX(0px);
  }
}
</style>