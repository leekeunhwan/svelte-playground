<script>
  export let todoText;
  export let handleTodolistAdd;
  export let handleTodoTextChange;
</script>

<form class="input" on:submit|preventDefault={handleTodolistAdd}>
  <input
    class="input__textInput"
    type="text"
    value={todoText}
    on:change={handleTodoTextChange}
  />
  <button class="input__submitBtn" on:onclick={handleTodolistAdd}>작성</button>
</form>

<style>
  .input {
    display: flex;
    align-items: center;
    width: 100%;
    height: 30px;
  }

  .input__textInput {
    flex: 1;
    height: 100%;
    border: 1px solid #cacaca;
    border-radius: 4px;
    box-sizing: border-box;
    padding-left: 8px;
  }

  .input__textInput:focus,
  .input__textInput:active {
    outline-color: #cacaca;
  }

  .input__submitBtn {
    margin-left: 8px;
    height: 100%;
    background-color: #ffffff;
    border: 1px solid #cacaca;
    border-radius: 4px;
    transition: 200ms all ease-in-out;
    cursor: pointer;
  }

  .input__submitBtn:active,
  .input__submitBtn:focus {
    outline: none;
  }

  .input__submitBtn:hover {
    background-color: #cacaca;
    color: #ffffff;
  }
</style>
