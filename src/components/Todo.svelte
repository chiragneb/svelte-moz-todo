<script>
  import { createEventDispatcher } from "svelte";
  const dispatch = createEventDispatcher();
  export let todo;
  let editing = false;
  let name = todo.name;

  function update(updatedTodo) {
    todo = { ...todo, ...updatedTodo };
    todo;
    dispatch("update", todo);
  }

  function onCancel() {
    name = todo.name;
    editing = false;
  }

  function onSave() {
    update({ name });
    editing = false;
  }

  function onRemove() {
    dispatch("remove", todo);
  }

  function onEdit() {
    editing = true;
  }

  function onToggle() {
    update({ completed: !todo.completed });
  }
</script>

<div class="stack-small">
  {#if editing}
    <form
      on:submit|preventDefault={onSave}
      class="stack-small"
      on:keydown={(e) => e.key === "Escape" && onCancel()}
    >
      <div class="form-group">
        <label for="todo-{todo.id}" class="todo-label"
          >New name for '{todo.name}'</label
        >
        <input
          bind:value={name}
          type="text"
          id="todo-{todo.id}"
          autoComplete="off"
          class="todo-text"
        />
      </div>
      <div class="btn-group">
        <button class="btn todo-cancel" on:click={onCancel} type="button">
          Cancel<span class="visually-hidden">renaming {todo.name}</span>
        </button>
        <button
          class="btn btn__primary todo-edit"
          type="submit"
          disabled={!name}
        >
          Save<span class="visually-hidden">new name for {todo.name}</span>
        </button>
      </div>
    </form>
  {:else}
    <div class="c-cb">
      <input
        type="checkbox"
        id="todo-{todo.id}"
        on:click={onToggle}
        checked={todo.completed}
      />
      <label for="todo-{todo.id}" class="todo-label">{todo.name}</label>
    </div>
    <div class="btn-group">
      <button type="button" class="btn" on:click={onEdit}>
        Edit <span class="visually-hidden">{todo.name}</span>
      </button>
      <button type="button" class="btn btn__danger" on:click={onRemove}>
        Delete <span class="visually-hidden">{todo.name}</span>
      </button>
    </div>
  {/if}
</div>
