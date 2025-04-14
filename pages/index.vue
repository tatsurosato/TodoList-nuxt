<script>
export default {
  data() {
    return {
      newTodoText: '',
      todos: [],
    }
  },
  methods: {
    addTodo() {
      if (!this.newTodoText) return alert('ToDoを入力してください')
      this.todos.push({
      isDone: false,
      text: this.newTodoText,
      })
        this.newTodoText = ''
      },
    delateTodo() {
      this.todos = this.todos.filter(todo => !todo.isDone)
    }
  },
}
</script>

<template>
  <h1>My ToDo App</h1>
  <input type="text" v-model="newTodoText" />
  <button @click="addTodo">追加</button>
  <button @click="delateTodo">完了済みを削除する</button>
  <p v-if="todos.length === 0">ToDoがまだありません！</p>
  
  <ul v-else>  
    <li v-for="todo in todos">
      <p class="userFrame">
        <input type="checkbox" v-model="todo.isDone" />
        <span :class="{ 'todo-done': todo.isDone }">{{ todo.text }}</span>
      </p>
    </li>
  </ul>
  
</template>

<style>
body {
  background-color: #eee;
}

.todo-done {
  text-decoration: line-through;
}

.userFrame {
  border-style: solid;
  border-color: rgb(72, 132, 253);
  background-color: #dceefe;
  display: inline-block;
}

</style>

