<template>
  <div class="app">
    <h1>My Todo List</h1>
    <div class="filter">
      <label for="filter">Filter:</label>
      <select id="filter" v-model="selectedFilter">
        <option value="all">All</option>
        <option value="completed">Completed</option>
        <option value="incomplete">Incomplete</option>
      </select>
    </div>
    <ul>
      <li v-for="(todo, index) in filteredTodos" :key="index">
        <div class="todo-item">
          <input type="checkbox" v-model="todo.completed" @change="updateTodo" />
          <span :class="{ 'completed': todo.completed }">{{ todo.text }}</span>
          <button class="delete" @click="deleteTodo(index)">x</button>
        </div>
      </li>
    </ul>
    <form class="add-todo" @submit.prevent="addTodo">
      <input type="text" v-model="newTodoText" placeholder="Add a new todo" />
      <button type="submit">Add</button>
    </form>
  </div>
</template>

<script>
export default {
  data() {
    return {
      todos: [],
      newTodoText: '',
      selectedFilter: 'all',
    };
  },
  computed: {
    filteredTodos() {
      if (this.selectedFilter === 'completed') {
        return this.todos.filter((todo) => todo.completed);
      } else if (this.selectedFilter === 'incomplete') {
        return this.todos.filter((todo) => !todo.completed);
      } else {
        return this.todos;
      }
    },
  },
  methods: {
    addTodo() {
      if (this.newTodoText.trim() !== '') {
        this.todos.push({
          text: this.newTodoText,
          completed: false,
        });
        this.newTodoText = '';
      }
    },
    deleteTodo(index) {
      this.todos.splice(index, 1);
    },
    updateTodo() {
      // no need to do anything here since we are using v-model
    },
  },
};
</script>

<style>
.app {
  max-width: 600px;
  margin: 0 auto;
}

h1 {
  text-align: center;
  font-size: 36px;
  margin-bottom: 20px;
}

ul {
  list-style: none;
  padding: 0;
  margin: 0;
}

.todo-item {
  display: flex;
  align-items: center;
  margin-bottom: 10px;
}

.todo-item input[type='checkbox'] {
  margin-right: 10px;
}

.todo-item .completed {
  text-decoration: line-through;
}

.add-todo {
  display: flex;
  margin-top: 20px;
}

.add-todo input[type='text'] {
  flex: 1;
  padding: 10px;
  font-size: 18px;
  border: none;
  border-radius: 5px 0 0 5px;
}

.add-todo button {
  padding: 10px 20px;
  font-size: 18px;
  background-color: #4caf50;
  color: #fff;
  border: none;
  border-radius: 0 5px 5px
} 
</style>