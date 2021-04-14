<script>
  export let todolist;
</script>

<ul class="list__container">
  {#each todolist as todo (todo.id)}
    <li class="list__item">{todo.text}</li>
  {/each}
</ul>

<style>
  .list__container {
    padding: 0 10px;
  }
  .list__item {
    display: flex;
    justify-content: space-between;
    list-style: none;
    padding-bottom: 8px;
  }
</style>
