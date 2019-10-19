<script>
  import TaskItem from "./TaskItem.svelte";

  let newTaskTitle = "";
  let newTaskNote = "";
  let newTaskAmount = "";
  let newTaskPrice = "";
  let currentFilter = "all";
  let nextId = 4;
  let tasks = [
    {
      id: 1,
      title: 'lorem 1',
      completed: false,
      note: 'lorem note',
      amount: '2',
      price: '22.99'
    },
    {
      id: 2,
      title: 'lorem 2',
      completed: false,
      note: 'lorem note',
      amount: '2',
      price: '22.99'
    },
    {
      id: 3,
      title: 'lorem 3',
      completed: false,
      note: 'lorem note',
      amount: '2',
      price: '22.99'
    }
  ];

  $: tasksRemaining = filteredTasks.filter(task => !task.completed).length;
  $: filteredTasks =
    currentFilter === "all"
      ? tasks
      : currentFilter === "completed"
      ? tasks.filter(task => task.completed)
      : tasks.filter(task => !task.completed);

  function addTask(event) {
    if (event.key === "Enter") {
      tasks = [
        ...tasks,
        {
          id: nextId,
          completed: false,
          title: newTaskTitle,
          note: newTaskNote,
          amount: newTaskAmount,
          price: newTaskPrice
        }
      ];
      console.log(
        {
          id: nextId,
          completed: false,
          title: newTaskTitle,
          note: newTaskNote,
          amount: newTaskAmount,
          price: newTaskPrice
        }
      )
      nextId = nextId + 1;
      newTaskTitle = "";
      newTaskNote = "";
      newTaskAmount = "";
      newTaskPrice = "";
    }
  }

  function checkAllTasks(event) {
    tasks.forEach(task => (task.completed = event.target.checked));
    tasks = tasks;
  }

  function updateFilter(newFilter) {
    currentFilter = newFilter;
  }

  function clearCompleted() {
    tasks = tasks.filter(task => !task.completed);
  }

  function handleDeleteTask(event) {
    tasks = tasks.filter(task => task.id !== event.detail.id);
  }
  function handleToggleComplete(event) {
    const taskIndex = tasks.findIndex(task => task.id === event.detail.id);
    const updatedTask = {
      ...tasks[taskIndex],
      completed: !tasks[taskIndex].completed
    };
    tasks = [
      ...tasks.slice(0, taskIndex),
      updatedTask,
      ...tasks.slice(taskIndex + 1)
    ];
  }
</script>

<style>
  .tasks_header {
    display: flex;
    flex-direction: row;
    align-items: center;
  }
  .logo {
    display: block;
    margin: 20px auto;
    width: 30%;
  }
  .tasks_container {
    max-width: 800px;
    margin: 10px auto;
  }
  .task-input {
    width: 45%;
    margin: 20px auto;
    padding: 10px, 20px;
    font-size: 18px;
    margin-bottom: 20px;
  }
  .inner-container {
    display: flex;
    align-items: center;
    justify-content: space-between;
    font-size: 16px;
    border-top: 1px solid lightgrey;
    padding-top: 15px;
    margin-bottom: 13px;
  }
  .task_form {
    display: flex;
    flex-direction: column;
    border: 0.5px solid lightgrey;
  }
  .inner-container-input {
    margin-right: 12px;
  }
  button {
    font-size: 14px;
    background-color: white;
    appearance: none;
  }
  button:hover {
    background: lightseagreen;
  }
  button:focus {
    outline: none;
  }
  .active {
    background: lightseagreen;
  }
  input::placeholder{
    font-size: 12px;
  }
</style>

<div class="tasks_container">
<header class="tasks_header">
  <a href="https://image.flaticon.com/icons/svg/138/138849.svg" target="_blank">
    <img src={'https://image.flaticon.com/icons/svg/138/138849.svg'} alt="svelte logo" class="logo" />
  </a>
  <h2>Svelte list</h2>
</header>
  <form class="task_form">
    <input
      type="text"
      class="task-input"
      placeholder="Insert task item ..."
      bind:value={newTaskTitle}
      on:keydown={addTask} />
    <input
      type="text"
      class="task-input"
      placeholder="Insert note ..."
      bind:value={newTaskNote}
      on:keydown={addTask} />
    <input
      type="text"
      class="task-input"
      placeholder="Insert amount ..."
      bind:value={newTaskAmount}
      on:keydown={addTask} />
    <input
      type="text"
      class="task-input"
      placeholder="Insert price ..."
      bind:value={newTaskPrice}
      on:keydown={addTask} />
    <!-- <button on:click={addTask}>add task item</button> -->
  </form>
  {#each filteredTasks as task}
    <div class="task-item">
      <TaskItem
        {...task}
        on:deleteTask={handleDeleteTask}
        on:toggleComplete={handleToggleComplete} />
    </div>
  {/each}
  <div class="inner-container">
    <div>
      <label>
        <input
          class="inner-container-input"
          type="checkbox"
          on:change={checkAllTasks} />
        Check All
      </label>
    </div>
    <div>{tasksRemaining} items left</div>
  </div>
  <div class="inner-container">
    <div>
      <button
        on:click={() => updateFilter('all')}
        class:active={currentFilter === 'all'}>
        All
      </button>
      <button
        on:click={() => updateFilter('active')}
        class:active={currentFilter === 'active'}>
        Active
      </button>
      <button
        on:click={() => updateFilter('completed')}
        class:active={currentFilter === 'completed'}>
        Completed
      </button>
    </div>
    <div>
      <button on:click={clearCompleted}>Clear Completed</button>
    </div>
  </div>
</div>
