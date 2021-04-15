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
    handleTodoInputClear();
  }

  function handleTodoInputClear() {
    todoText = "";
  }

  function handleTodoDoneUpdate(id) {
    const targetIndex = todolist.findIndex((list) => list.id === id);
    todolist[targetIndex].done = !todolist[targetIndex].done;
  }

  function handleTodoDelete(id) {
    const updatedTodolist = todolist.filter((list) => list.id !== id);
    todolist = updatedTodolist;
  }
</script>

<div class="form__container">
  <TodoInput {handleTodolistAdd} {handleTodoTextChange} {todoText} />
  <TodoList {todolist} {handleTodoDoneUpdate} {handleTodoDelete} />
</div>

<style>
  .form__container {
    padding: 20px;
  }
</style>
