<script>
import StatusFiletr from './components/StatusFilter.vue'
import TodoItem from './components/TodoItem.vue'

export default {
  components: {
    StatusFiletr,
    TodoItem,
  },
  data() {
    let todos = []
    const jsonData = localStorage.getItem('todos') || '[]'
    try {
      todos = JSON.parse(jsonData)
    } catch (e) {
      console.log(e)
    }

    return {
      todos,
      title: '',
      status: 'all',
    }
  },
  computed: {
    activeTodos() {
      return this.todos.filter(todo => !todo.completed)
    },
  },
  watch: {
    todos: {
      deep: true,
      handler() {
        localStorage.setItem('todos', JSON.stringify(this.todos))
      },
    },
  },
  methods: {
    handleSubmit() {
      this.todos.push({
        id: Date.now(),
        title: this.title,
        completed: false,
      })
      this.title = ''
    },
  },
}
</script>

<template>
  <div class="todoapp">
    <h1 class="todoapp__title">todos</h1>

    <div class="todoapp__content">
      <header className="todoapp__header">
        <button
          type="button"
          class="todoapp__toggle-all"
          :class="{ active: activeTodos.length === 0 }"
          data-cy="ToggleAllButton"
        ></button>

        <form @submit.prevent="handleSubmit">
          <input
            data-cy="NewTodoField"
            type="text"
            className="todoapp__new-todo"
            placeholder="What needs to be done?"
            v-model="title"
          />
        </form>
      </header>

      <section class="doapp__main" data-cy="TodoList">
        <TodoItem
          v-for="(todo, index) of todos"
          :key="todo.id"
          :todo="todo"
          @update="todos[index] = $event"
          @delete="todos.splice(index, 1)"
        />
      </section>

      <footer class="todoapp__footer" data-cy="Footer">
        <span class="todo-count" data-cy="TodosCounter">
          {{ activeTodos.length }} items left
        </span>

        <StatusFiletr v-model="status" />

        <button
          type="button"
          class="todoapp__clear-completed"
          data-cy="ClearCompletedButton"
          v-if="activeTodos.length > 0"
        >
          Clear completed
        </button>
      </footer>
    </div>
    <!-- <div
      data-cy="ErrorNotification"
      class="notification is-danger is-light has-text-weight-normal"
    >
      <button data-cy="HideErrorButton" type="button" class="delete" />
      <br />
      Title should not be empty
      <br />
      Unable to add a todo
      <br />
      Unable to delete a todo
      <br />
      Unable to update a todo
    </div> -->
  </div>
</template>
