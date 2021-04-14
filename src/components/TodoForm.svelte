<script>
  import TodoInput from "./TodoInput.svelte";
  import TodoList from "./TodoList.svelte";

  let todoText = "";
  let todolist = [];

  function handleTodoTextChange(text) {
    todoText = text;
  }
</script>

<div class="form__container">
  <TodoInput on:onChange={handleTodoTextChange} {todoText} />
  <TodoList />
</div>

<style>
  .form__container {
    padding: 20px;
  }
</style>
