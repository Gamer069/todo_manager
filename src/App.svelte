<script>
    import { onMount } from 'svelte';
    import { menu } from './stores.js';
    import List from './List.svelte';

    console.log("App.svelte");

    let todos = []
    let latest = '';

    onMount(() => {
        const saved = localStorage.getItem('nativetodolist');
        todos = JSON.parse(saved) || [];
    });

    const appendTodo = () => {
        latest = latest.trim();
        if (latest != '') {
            todos = [...todos, latest];
            localStorage.setItem('nativetodolist', JSON.stringify(todos));
            latest = '';
        }
    };

    const removeTodo = (i) => {
        todos.splice(i, 1);
        localStorage.setItem('nativetodolist', JSON.stringify(todos));
        location.reload();
    };

    const wipeTodos = () => {
        todos = [];
        localStorage.setItem('nativetodolist', '');
    };

    const listView = () => {
        menu.set(true);
    }

    window.addEventListener('keydown', (event) => {
        console.log(event.key);
        if (event.ctrlKey && event.key === 'i') {
            event.preventDefault();
            wipeTodos();
        }
        if (event.ctrlKey && event.key == "'") {
            event.preventDefault();
            listView();
        }
        if (event.key == 'Escape') {
            event.preventDefault();
            location.reload();
        }
    });

    let menuValue = false;

    menu.subscribe((val) => {
        menuValue = val;
    });
</script>

<main>
    {#if menuValue}
        <List {todos}/>
    {:else}
        <ul>
            {#each todos as todo, i}
                <li class="bold">{todo} <button on:click={() => removeTodo({i})}>❌</button></li>
            {/each}
        </ul>

        <form on:submit|preventDefault={appendTodo}>
            <input bind:value={latest} autofocus>
            <input type="submit" value="Add todo...">
        </form>

        <button on:click={wipeTodos}>Wipe</button>
        <button on:click={listView}>List</button>
    {/if}
</main>
