<script>
import { ref } from 'vue';
import VueDatePicker from '@vuepic/vue-datepicker';
import '@vuepic/vue-datepicker/dist/main.css';
import { ja } from 'date-fns/locale';

const LOCAL_STORAGE_KEY = 'todos'; // localStorageのキーを定数化

export default {
  data() {
    return {
      newTodoText: '',
      todos: [],
    };
  },
  created() {
    if (process.client) { // クライアントサイドでのみ実行
      const savedTodos = localStorage.getItem(LOCAL_STORAGE_KEY);
      if (savedTodos) {
        this.todos = JSON.parse(savedTodos);
        this.todos.forEach(todo => {
          if (todo.dueDate) {
            todo.dueDate = new Date(todo.dueDate);
          }
        });
      }
    }
  },
  watch: {
    todos: {
      handler(newTodos) {
        if (process.client) { // クライアントサイドでのみ実行
          localStorage.setItem(LOCAL_STORAGE_KEY, JSON.stringify(newTodos));
        }
      },
      deep: true,
    },
  },
  methods: {
    addTodo() {
      if (!this.newTodoText) {
        return alert('ToDoを入力してください');
      }
      this.todos.push({
        isDone: false,
        text: this.newTodoText,
        dueDate: null,
      });
      this.newTodoText = '';
      if (process.client) { // クライアントサイドでのみ実行
        localStorage.setItem(LOCAL_STORAGE_KEY, JSON.stringify(this.todos));
      }
    },
    delateTodo() {
      this.todos = this.todos.filter(todo => !todo.isDone);
      if (process.client) { // クライアントサイドでのみ実行
        localStorage.setItem(LOCAL_STORAGE_KEY, JSON.stringify(this.todos));
      }
    },
  },
  components: { VueDatePicker },
  setup() {
    return {
      ja,
    };
  },
};
</script>

<template>
  <div class="background">
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

          <VueDatePicker
            v-model="todo.dueDate"
            :format-locale="ja"
            format="yyyy-MM-dd"
            :enable-time-picker="false"
            auto-apply
          />
          <input type="text" placeholder="メモ">
          

        </p>
      </li>
    </ul>
  </div>
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
  border-color: rgb(253, 78, 72);
  background-color: #fedcdc;
  display: inline-block;
}
.background {
  background-image: url('/images/sabaku.jpg');
  background-size: cover;
  background-position: center;
  height: 100vh;
  width: 100%;
}

</style>

