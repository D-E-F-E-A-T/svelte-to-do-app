<script>
    import { createEventDispatcher } from 'svelte';
    import { fly } from 'svelte/transition';
    
    export let id;
    export let title;
    export let completed;

    const dispatch = createEventDispatcher();
    
    function deleteTodo() {
        dispatch('deleteTodo', {
            id: id
        });
    }

    function toggleComplete() {
        dispatch('toggleComplete', {
            id: id
        });
    }
</script>

<style>
    .todo-item {
        display: flex;
        align-items: center;
        justify-content: space-between;
        margin-bottom: 8px;
    }

    .todo-item .todo-item-label {
        color: rgba(0, 0, 0, 0.8);
        transition: all ease 0.7s;
    }

    .todo-item.completed .todo-item-label {
        text-decoration: line-through;
        color: #BBB;
    }

    .todo-item-left {
        display: flex;
        align-items: center;
        justify-content: start;
    }

    .todo-item-left input {
        margin-right: 10px;
    }

    .todo-item .remove-item {
        cursor: pointer;
        color: rgba(0, 0, 0, 0.8);
        transition: color ease 0.5s;
        margin-right: 10px;
    }
    .todo-item .remove-item:hover {
        color: #CCC;
    }
</style>

<div class="todo-item" class:completed={completed}>
    <div class="todo-item-left" transition:fly="{{y: 20, duration: 300 }}">
        <input type="checkbox" bind:checked={completed} on:change={toggleComplete} />
        <div class="todo-item-label">{title}</div>
    </div>
    <div class="remove-item" on:click={deleteTodo}>X</div>
</div>