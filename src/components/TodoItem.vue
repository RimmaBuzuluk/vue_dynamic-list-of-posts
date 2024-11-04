<script>
export default {
  name: 'TodoItem',
  props: {
    todo: Object,
  },
  emits: ['update', 'delete'],
  methods: {
    toggle() {
      this.$emit('update', {
        ...this.todo,
        completed: !this.todo.completed,
      })
    },
    remove() {
      this.$emit('delete')
    },
  },
}
</script>

<template>
  <div data-cy="Todo" class="todo" :class="{ completed: todo.completed }">
    <label class="todo__status-label">
      <input
        data-cy="TodoStatus"
        type="checkbox"
        class="todo__status"
        :checked="todo.completed"
        @change="toggle"
      />
    </label>

    <form v-if="false">
      <input
        data-cy="TodoTitleField"
        type="text"
        class="todo__title-field"
        placeholder="Empty todo will be deleted"
        value="Todo is being edited now"
      />
    </form>
    <template v-else>
      <span data-cy="TodoTitle" class="todo__title">
        {{ todo.title }}
      </span>

      <button
        type="button"
        class="todo__remove"
        data-cy="TodoDelete"
        @click="remove"
      >
        ×
      </button>
    </template>

    <div
      data-cy="TodoLoader"
      class="modal overlay"
      :class="{ 'is-active': false }"
    >
      <!-- <div clasclasssName="modal-background has-background-white-ter" />
      <div class="loader" /> -->
    </div>
  </div>
</template>
<style></style>
