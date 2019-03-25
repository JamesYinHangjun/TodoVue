<template>
<div class="todo-container">
  <div class="todo-wrap">
    <TodoHeader :addTodo="addTodo"/>
    <TodoList :todos="todos" :deleteTodo="deleteTodo"/>
    <TodoFooter :todos="todos" :deleteCompleteTodos="deleteCompleteTodos" :seleteAllTodos="seleteAllTodos"/>
  </div>
</div>
</template>

<script>
import TodoHeader from './components/TodoHeader'
import TodoList from './components/TodoList'
import TodoFooter from './components/TodoFooter'

export default {
  data() {
      return {
          // todos: [
          //     {
          //         title:'吃饭',complete: false
          //     },
          //     {
          //         title:'睡觉',complete: true
          //     },
          //     {
          //         title:'敲代码',complete: false
          //     }
          // ]

          // 从localStorage 中 读取数据
          todos: JSON.parse(window.localStorage.getItem('todo_key') || '[]')
      }
  },
  methods: {
    addTodo(todo) {
        this.todos.unshift(todo)
    },
    // 点击按钮删除
    deleteTodo(index) {
        this.todos.splice(index,1)
    },

    // 删除选中(complete是true的)的todo.这个过程中没有涉及到属性的变化，所有不用传递参数
    deleteCompleteTodos() {
        // 过滤剩下 complete 是false 的 todo
        // 过滤产生一个新数组，赋值给原来的 todos
        this.todos = this.todos.filter(todo => !todo.complete)
    },

    // 这个过程中,当选中了全选的框时，涉及到了所有todo的complete变化，所以传递一个参数
    seleteAllTodos(check) {
        // 遍历 todos,改变每个 todo 的 complete
        this.todos.forEach(todo => todo.complete = check)
    }
  },

  watch: {      // 监视
      todos: {
          deep: true,          // 深度监视
          handler: function(value) {
              window.localStorage.setItem('todo_key',JSON.stringify(value))
          }
      }
  },

  components: {
    TodoHeader,
    TodoList,
    TodoFooter
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
