<script>
  let todoInput = "";
  let todos = [
    { id: 0, text: "Test1", done: false },
    { id: 1, text: "Test2", done: false },
    { id: 2, text: "Test3", done: false },
  ];

  $: doneTodos = todos.filter((t) => t.done);
  $: undoneTodos = todos.filter((t) => !t.done);

  const addTodo = () => {
    todos.push({ id: Math.random(), text: todoInput, done: false });
    todos = todos;

    todoInput = "";
  };

  const checkTodo = (checked, id) => {
    todos = todos.map((t) => {
      if (id === t.id) t.done = checked;
      return t;
    });
  };
</script>

<div>
  <h1>Todo App mit Svelte</h1>
  <input type="text" bind:value={todoInput} placeholder="Neues Todo" />
  <input type="button" on:click={addTodo} value="Hinzufügen" />
  <ul>
    {#each undoneTodos as { id, text, done } (id)}
      <li class="listItem">
        <input
          on:change={checkTodo(event.target.checked, id)}
          bind:checked={done}
          type="checkbox"
        />
        {text}
      </li>
		{:else}
			<p>Alles erledigt! 🥳</p>
    {/each}
  </ul>
  <hr />
  <ul>
    {#each doneTodos as { id, text, done } (id)}
      <li class="listItem">
        <input
          on:change={checkTodo(event.target.checked, id)}
          bind:checked={done}
          type="checkbox"
        />
        <s>{text}</s>
      </li>
		{:else}
			<p>Noch nichts fertig 🙁</p>
    {/each}
  </ul>
</div>

<style>
  :global(body) {
    padding: 1rem;
  }

  .listItem {
    display: flex;
    flex-direction: row;
    align-items: center;
    gap: 0.5rem;
  }
  .listItem input {
    margin: 0;
  }
</style>
