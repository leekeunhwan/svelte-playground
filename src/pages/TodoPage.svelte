<script>
  import TodoHeader from "../components/TodoHeader.svelte";
  import TodoForm from "../components/TodoForm.svelte";
</script>

<div class="page__container">
  <TodoHeader />
  <TodoForm />
</div>

<style>
  .page__container {
    background: linear-gradient(180deg, #3dc5ae, #25b1a4);
  }
</style>
