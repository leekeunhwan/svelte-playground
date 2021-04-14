<script>
  import { createEventDispatcher } from "svelte";

  export let todoText;

  const dispatch = createEventDispatcher();

  function handleInputChange(e) {
    console.log("event", e.target.value);
    dispatch("handleTodoTextChange", {
      text: e.target.value,
    });
  }
</script>

<div class="input">
  <input
    class="input__textInput"
    type="text"
    value={todoText}
    on:change={handleInputChange}
  />
  <button class="input__submitBtn">작성</button>
</div>

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
