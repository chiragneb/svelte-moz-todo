<script> 
  import { createEventDispatcher } from 'svelte'
  const dispatch = createEventDispatcher()
  
  import FilterButton from './FilterButton.svelte'
  import Todo from './Todo.svelte'

  export let todos = []
  $: totalTodos = todos.length;
  $: completedTodos = todos.filter((todo) => todo.completed).length;

  function removeTodo(todo) {
    todos = todos.filter((t) => t.id !== todo.id)
  }

  let newTodoName = ''
  $: console.log('newTodoName: ', newTodoName)

  function addTodo() {
   todos = [...todos, {id: newTodoId, name: newTodoName, completed: false 
   }]
    newTodoName = ''
  }

  let newTodoId 
     $: {
        if (totalTodos === 0) {
            newTodoId = 1;
        } else {
            newTodoId = Math.max(...todos.map((t) => t.id)) + 1;
        }
     }

   let filter = 'all'
   const filterTodos = (filter, todos) => 
      filter === 'active' ? todos.filter((t) => !t.completed) :
      filter === 'completed' ? todos.filter((t) => t.completed) :
      todos

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

  <FilterButton bind:filter={filter} />

  <!-- TodosStatus -->
  <h2 id="list-heading">{completedTodos} out of {totalTodos} items completed</h2>


  
<ul role="list" class="todo-list stack-large" aria-labelledby="list-heading">
  {#each filterTodos(filter, todos) as todo (todo.id)}
  <li class="todo">
    <Todo {todo} />
  </li>
  {:else}
  <li>Nothing to do here!</li>
  {/each}
</ul>


  <hr />

  <!-- MoreActions -->
  <div class="btn-group">
    <button type="button" class="btn btn__primary">Check all</button>
    <button type="button" class="btn btn__primary">Remove completed</button>
  </div>
</div>
