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
        justify-content: start;
        margin-bottom: 8px;
    }

    .todo-item-check {
        flex: 1;
        display: flex;
        align-items: center;
        justify-content: start;
    }

    .todo-item-check input[type="checkbox"] {
        position: absolute;
        opacity: 0;
        pointer-events: none;
    }

    .todo-item-check span {
        position: relative;
        padding-left: 35px;
        cursor: pointer;
        display: inline-block;
        width: 100%;
        height: 25px;
        line-height: 25px;
        font-size: 1rem;
        user-select: none;
        color: #676767;
        padding-bottom: 5px;
        margin-bottom: 5px;
    }

    .todo-item-check input[type="checkbox"] + span::before {
        content: "";
        position: absolute;
        top: 0;
        left: 0;
        width: 18px;
        height: 18px;
        border: 2px solid#676767;
        border-radius: 50%;
        margin-top: 2px;
        transition: .3s ease;
    }

    .todo-item-check input[type="checkbox"]:checked + span::before {
        border-color: #5a73e0;
        border-width: 4px;
        border-left-color: transparent;
        border-top-color: transparent;
        border-radius: 0;
        width: 14px;
        margin-top: -6px;
        transform: rotate(40deg);
    }

    .todo-item-check input[type="checkbox"] + span::after {
        content: "";
        position: absolute;
        left: 35px;
        right: 0;
        height: 1px;
        top: 100%;
        background: #676767;
        transition: .3s ease;
    }

    .todo-item-check input[type="checkbox"]:checked + span {
        color: #CCC;
    }

    .todo-item-check input[type="checkbox"]:checked + span::after {
        top: 50%;
        background: #CCC;
    }

    .todo-item .remove-item {
        cursor: pointer;
        color: #676767;
        font-size: 1.4rem;
        padding: 5px;
        display: flex;
        align-items: center;
        justify-content: center;
        transition: all ease 0.5s;
        margin-left: 10px;
        opacity: 0;
    }
    .todo-item:hover .remove-item {
        opacity: 1;
    }
    .todo-item .remove-item:hover {
        color: #5a73e0;
    }

    @media screen and (max-width: 600px) {
        .todo-item .remove-item {
            opacity: .7;
        }
    }
</style>

<div class="todo-item" class:completed={completed}>
    <label class="todo-item-check" transition:fly="{{y: 20, duration: 300 }}">
        <input type="checkbox" bind:checked={completed} on:change={toggleComplete} />
        <span class="todo-item-label">{title}</span>
    </label>
    <div class="remove-item" on:click={deleteTodo}>x</div>
</div>