<script>
  import FilterButton from "./FilterButton.svelte";
  import Todo from "./Todo.svelte";
  import MoreActions from "./MoreActions.svelte";

  export let todos = [];
  $: totalTodos = todos.length;
  $: completedTodos = todos.filter((todo) => todo.completed).length;

  function removeTodo(todo) {
    todos = todos.filter((t) => t.id !== todo.id);
  }

  let newTodoName = "";
  $: console.log("newTodoName: ", newTodoName);

  function addTodo() {
    todos = [...todos, { id: newTodoId, name: newTodoName, completed: false }];
    newTodoName = "";
  }

  let newTodoId;
  $: {
    if (totalTodos === 0) {
      newTodoId = 1;
    } else {
      newTodoId = Math.max(...todos.map((t) => t.id)) + 1;
    }
  }

  let filter = "all";
  const filterTodos = (filter, todos) =>
    filter === "active"
      ? todos.filter((t) => !t.completed)
      : filter === "completed"
      ? todos.filter((t) => t.completed)
      : todos;

  function updateTodo(todo) {
    const i = todos.findIndex((t) => t.id === todo.id);
    todos[i] = { ...todos[1], ...todo };
  }

  const checkAllTodos = (completed) => {
    todos.forEach((t) => (t.completed = completed));
    todos = todos;
  };
  const removeCompletedTodos = () =>
    (todos = todos.filter((t) => !t.completed));
</script>

<!-- Todos.svelte -->
<div class="todoapp stack-large">
  <!-- NewTodo -->
  <form on:submit|preventDefault={addTodo}>
    <h2 class="label-wrapper">
      <label for="todo-0" class="label__lg"> What needs to be done? </label>
    </h2>
    <input
      bind:value={newTodoName}
      type="text"
      id="todo-0"
      autocomplete="off"
      class="input input__lg"
    />
    <button type="submit" disabled="" class="btn btn__primary btn__lg">
      Add
    </button>
  </form>

  <FilterButton bind:filter />

  <!-- TodosStatus -->
  <h2 id="list-heading">
    {completedTodos} out of {totalTodos} items completed
  </h2>

  <!-- svelte-ignore a11y-no-redundant-roles -->
  <ul role="list" class="todo-list stack-large" aria-labelledby="list-heading">
    {#each filterTodos(filter, todos) as todo (todo.id)}
      <li class="todo">
        <Todo
          {todo}
          on:update={(e) => updateTodo(e.detail)}
          on:remove={(e) => removeTodo(e.detail)}
        />
      </li>
    {:else}
      <li>Nothing to do here!</li>
    {/each}
  </ul>

  <hr />

  <!-- MoreActions -->
  <MoreActions
    {todos}
    on:checkAll={(e) => checkAllTodos(e.detail)}
    on:removeCompleted={removeCompletedTodos}
  />
</div>
