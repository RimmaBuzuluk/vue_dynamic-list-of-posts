<template>
  <div class="todoapp">
    <h1 class="todoapp__title">todos</h1>

    <div class="todoapp__content">
      <!-- <Header
        todos="{todos}"
        handleQueryChange="{handleQueryChange}"
        updateCompleted="{updateCompleted}"
        query="{query}"
        handleAddTodo="{handleAddTodo}"
        inputRef="{inputRef}"
        isLoading="{isLoading}"
      />

      <ListComponent
        todos="{tasks}"
        handleDelete="{handleDelete}"
        isLoadingTodos="{isLoadingTodos}"
        isLoading="{isLoading}"
        tempTodo="{tempTodo}"
        updateCompleted="{updateCompleted}"
        updateTitle="{updateTitle}"
      />
      {todos.length > 0 && (
      <Footer
        onClearCompleted="{deleteAllCompleted}"
        setFilterBy="{setFilterBy}"
        todos="{todos}"
        filterBy="{filterBy}"
      />
      )}-->
    </div>
    <Error errorMessage="{errorMessage}" setErrorMessage="{setErrorMessage}" />
  </div>
</template>
