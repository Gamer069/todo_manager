<script>
    import { menu } from './stores.js';
    import { maximized } from './stores.js';
    import MaximizedTodo from './MaximizedTodo.svelte';
    export let todos;

    console.log("List.svelte");
    
    let latest;

    let maximizedValue;

    maximized.subscribe((val) => {
        maximizedValue = val;
    });

    const viewTodo = (todo) => {
        latest = todo;
        // annoying but useful.
        maximized.set(true);
    }
</script>

<main>
    {#if maximizedValue}
        <MaximizedTodo todo={latest}/>
    {:else}
        <ul>
            {#each todos as todo}
                <button on:click={() => viewTodo(todo)}>{todo}</button>
            {/each}
        </ul>
    {/if}
</main>
