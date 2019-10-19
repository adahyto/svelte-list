<script>
  import { createEventDispatcher } from "svelte";
  import { fly } from "svelte/transition";
  export let id;
  export let title;
  export let note;
  export let amount;
  export let price;
  export let completed;
  const dispatch = createEventDispatcher();
  function deleteTask() {
    dispatch("deleteTask", {
      id: id
    });
  }
  function toggleComplete() {
    dispatch("toggleComplete", {
      id: id
    });
  }
</script>

<style>
  .task_item {
    margin-bottom: 15px;
    display: flex;
    align-items: center;
    justify-content: space-between;
    animation-duration: 0.3s;
  }
  .item-remove {
    cursor: pointer;
    margin-left: 15px;
  }
  .item-remove:hover {
    color: lightseagreen;
  }
  .task_item-left {
    display: flex;
    align-items: center;
  }
  .task_item_label {
    border: 1px solid white;
    margin-left: 12px;
  }
  .completed {
    text-decoration: line-through;
    color: grey;
  }
</style>

<div class="task_item">
  <div class="task_item-left" transition:fly={{ y: 20, duration: 300 }}>
    <input
      type="checkbox"
      bind:checked={completed}
      on:change={toggleComplete} />
    <div class="task_item_label" class:completed>{title}</div>
    <div class="task_item_label" class:completed>{note}</div>
    <div class="task_item_label" class:completed>{amount}</div>
    <div class="task_item_label" class:completed>{price}</div>
  </div>
  <div class="item-remove" on:click={deleteTask}>×</div>
</div>
