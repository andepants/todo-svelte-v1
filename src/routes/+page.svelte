<script lang='ts'>
  let newToDo = '';
  let toDos = ['Task 1', 'Task 2', 'Task 3'];

  interface DeleteToDoEvent extends Event {
    key : string;
  }

  interface indexObject {
    detail : number;
  }

  function updateToDo (event : indexObject) {
    const index = event.detail;
    toDos[index] = 'UPDATED TASK';
  }

  function deleteToDo(event : indexObject) {
    const index = event.detail;
    toDos = toDos.filter((_, i) => i !== index);
  }

  function handleKeyDown(event : DeleteToDoEvent, index : number) {
    if (event.key === 'Enter' || event.key === ' ') {
      deleteToDo({
        detail: index
      });
    }
  }

  function handleClick() {
    toDos = [...toDos, newToDo];
    newToDo = '';
  }

</script>

<h1>My To Do List</h1>

<input bind:value={newToDo} placeholder="Add a ToDo">
<button on:click|preventDefault={handleClick}>Add</button>

<ul>
  {#each toDos as toDo, index}
    <li>{toDo}</li>
    <button on:click|preventDefault={() => deleteToDo({ detail: index })} on:keydown={event => handleKeyDown(event, index)}>Delete</button>
    <button on:click|preventDefault={() => updateToDo({ detail: index })}>Update</button>
  {/each}
</ul>
