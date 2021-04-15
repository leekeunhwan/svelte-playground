<script>
  export let todolist;
  export let handleTodoDoneUpdate;
  export let handleTodoDelete;
</script>

<ul class="list__container">
  {#each todolist as todo (todo.id)}
    <li class="list__item">
      <div class={todo.done ? "list__text done" : "list__text"}>
        {todo.text}
      </div>
      <div class="list__buttons">
        <button on:click={() => handleTodoDoneUpdate(todo.id)}>완료</button>
        <button>수정</button>
        <button on:click={() => handleTodoDelete(todo.id)}>삭제</button>
      </div>
    </li>
  {/each}
</ul>

<style>
  .list__container {
    padding: 0 5px;
  }
  .list__item {
    display: flex;
    justify-content: space-between;
    list-style: none;
    padding-bottom: 8px;
  }

  .list__text {
    color: #ffffff;
    text-decoration: none;
  }

  .list__text.done {
    text-decoration: line-through;
  }

  .list__buttons {
    display: flex;
  }

  .list__buttons button {
    margin-right: 4px;
  }

  .list__buttons button:last-of-type {
    margin: 0;
  }
</style>
