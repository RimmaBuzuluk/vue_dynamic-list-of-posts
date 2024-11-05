<script>
import { deleteTodo, getTodos, postTodo, updateTodo } from './api/todos'
import StatusFiletr from './components/StatusFilter.vue'
import TodoItem from './components/TodoItem.vue'

export default {
  components: {
    StatusFiletr,
    TodoItem,
  },
  data() {
    return {
      todos: [],
      title: '',
      status: 'all',
    }
  },
  computed: {
    activeTodos() {
      return this.todos.filter(todo => !todo.completed)
    },
    completedTodos() {
      return this.todos.filter(todo => todo.completed)
    },
    visibleTodos() {
      switch (this.status) {
        case 'active':
          return this.activeTodos
        case 'completed':
          return this.completedTodos
        default:
          return this.todos
      }
    },
  },

  mounted() {
    getTodos().then(({ data }) => {
      this.todos = data
    })
  },
  methods: {
    handleSubmit() {
      postTodo(this.title).then(({ data }) => {
        this.todos.push(data)
      })
      this.title = ''
    },
    updateTodo({ id, title, completed }) {
      updateTodo({ id, title, completed }).then(({ data }) => {
        this.todos = this.todos.map(todo => (todo.id !== id ? todo : data))
      })
    },
    deleteTodo(todoId) {
      deleteTodo(todoId).then(() => {
        this.todos = this.todos.filter(todo => todo.id !== todoId)
      })
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

      <TransitionGroup
        name="list"
        tag="section"
        class="doapp__main"
        data-cy="TodoList"
      >
        <TodoItem
          v-for="todo of visibleTodos"
          :key="todo.id"
          :todo="todo"
          @update="updateTodo"
          @delete="deleteTodo(todo.id)"
        />
      </TransitionGroup>

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

<style>
.list-enter-active,
.list-leave-active {
  max-height: 60px;
  transition: all 0.5s ease;
}
.list-enter-from,
.list-leave-to {
  opacity: 0;
  max-height: 0;
  transform: scaleY();
}
</style>
