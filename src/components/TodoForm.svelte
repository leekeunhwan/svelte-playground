<script>
  import TodoInput from "./TodoInput.svelte";
  import TodoList from "./TodoList.svelte";
  import { generateUUID } from "../utils/generator";

  let todoText = "";
  let todolist = [];

  function handleTodoTextChange(event) {
    todoText = event.target.value;
  }

  function handleTodolistAdd() {
    const newTodo = {
      id: generateUUID(),
      text: todoText,
      done: false,
    };

    todolist = [...todolist, newTodo];
  }
</script>

<div class="form__container">
  <TodoInput {handleTodolistAdd} {handleTodoTextChange} {todoText} />
  <TodoList {todolist} />
</div>

<style>
  .form__container {
    padding: 20px;
  }
</style>
