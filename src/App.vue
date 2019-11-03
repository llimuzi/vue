<template>
  <div class="todo-container">
    <div class="todo-wrap">
      <Header :addTodo="addTodo"/>
      <List :todos="todos" :deleteTodo="deleteTodo" :updatedTodo="updatedTodo"/>
      <!-- <Footer :todos="todos" :selectAll="selectAll" :clearAllComplete:"clearAllComplete"/> -->
      <Footer :todos="todos" :selectAll="selectAll" :clearAllComplete="clearAllComplete"/>
  </div>
  </div>
     
</template>

<script>
import Header from './components/Header'
import List from './components/List'
import Footer from './components/Footer'
export default {
  data() {
  return{ 
    todos:[],
    xxx: 2
  }
},
  mounted(){
    setTimeout(()=>{
      //读取local保存todos数据
    const todos=JSON.parse(localStorage.getItem('todos_key')||'[]')
      //更新
      this.todos=todos
    },1000);
  },
  watch:{
    todos:{
      deep:true,
      handler:(value)=>{
        localStorage.setItem('todos_key',JSON.stringify(value))
      }
    }
  },

methods:{
  //增加
  addTodo(todo){
  this.todos.unshift(todo)
  },
  //删除
  deleteTodo(id){
    this.todos=this.todos.filter(todo=>id!==todo.id)
  },
  //全选/全不全
    selectAll(check){
        this.todos.forEach(todo=>todo.complete=check)
  },
  clearAllComplete () {
        this.todos = this.todos.filter(todo => !todo.complete)
      }
},
//更新todo的complete值
  updatedTodo(todo,isCheck){
      todo.complete=isCheck
  },
  components: { // 定义局部组件
    Header,
    List,
    Footer
  }
}
</script>

<style>
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
