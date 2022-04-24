<template>
  <transition name="todo" appear>
    <li>
    <label>
      <input type="checkbox" :checked="todoObj.done" @change="handleCheck(todoObj.id)"/>
      <span v-show="!todoObj.isEdit">{{todoObj.title}}</span>
      <input 
      v-show="todoObj.isEdit" 
      type="text" 
      :value="todoObj.title" 
      @blur="handleBlur($event,todoObj)"
      @keyup.enter="handleBlur($event,todoObj)"
      ref="input"
      >
    </label>
    <button class="btn btn-danger" @click="handleDelete(todoObj.id)">删除</button>
    <button class="btn btn-edit" @click="handleEdit(todoObj)" v-show="!todoObj.isEdit">编辑</button>
  </li>
  </transition>
</template>

<script>
    export default {
        name: 'TodoItem',
        props:['todoObj'],
        methods: {
          handleCheck(id){
            //通知app将对应done值取反
            this.$bus.$emit('checkedTodo',id);
          },
          handleDelete(id){
            if(confirm("确定删除吗？")){
              this.$bus.$emit('deleteTodo',id);
            }
          },
          handleEdit(todoObj){
            if(todoObj.hasOwnProperty.call('isEdit')){
              todoObj.isEdit = true;
            }else{
              this.$set(todoObj,'isEdit',true);
            }
            this.$nextTick(function(){
              this.$refs.input.focus();
            })
          },
          handleBlur(e,todoObj){
            todoObj.isEdit = false;
            if(!e.target.value.trim()) return alert("输入不能为空！");
            this.$bus.$emit('updateTodo',todoObj.id,e.target.value)
          }
        },
        //处理修改框自动获取焦点--使用自定义指令
        // directives: {
        //   focus: {
        //     update(el){
        //       el.focus();
        //     }
        //   }
        // }
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