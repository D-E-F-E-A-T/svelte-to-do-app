<script>
    import { slide, scale, fly } from 'svelte/transition';
    import TodoItem from './TodoItem.svelte';

    let newTodoTitle = '';
    let currentFilter = 'all';
    let nextId = 4;
    let addingNewItem = false;
    let addingNewItemChar = '+';

    let todos = [
        {
            id: 1,
            title: 'Primeiro Item',
            completed: false,
        },
        {
            id: 2,
            title: 'Segundo Item',
            completed: false,
        },
        {
            id: 3,
            title: 'Terceiro Item',
            completed: false,
        },
    ];
    
    function addTodo(event) {
        if (event.key === 'Enter') {
            todos = [...todos, {
                id: nextId,
                title: newTodoTitle,
                completed: false,
            }];
            nextId = nextId + 1;
            newTodoTitle = '';
        }
    }

    $: todosRemaining = filteredTodos.filter(todo => !todo.completed).length;
    $: filteredTodos = currentFilter === 'all' ? todos : currentFilter === 'completed'
        ? todos.filter(todo => todo.completed)
        : todos.filter(todo => !todo.completed);
        
    function checkAllTodos(event) {
        todos.forEach(todo => todo.completed = event.target.checked);
        todos = todos;
    }

    function updateFilter(newFilter) {
        currentFilter = newFilter;
    }

    function clearCompleted() {
        todos = todos.filter(todo => !todo.completed);
    }

    function handleDeleteTodo(event) {
        todos = todos.filter(todo => todo.id !== event.detail.id);
    }

    function handleToggleComplete(event) {
        const todoIndex = todos.findIndex(todo => todo.id === event.detail.id);
        const updatedTodo = {...todos[todoIndex], completed: !todos[todoIndex].completed}
        todos = [
            ...todos.slice(0, todoIndex),
            updatedTodo,
            ...todos.slice(todoIndex + 1),
        ];
    }

    function handleAddClickButton(event) {
        addingNewItem = !addingNewItem;
        addingNewItemChar = addingNewItem ? '-' : '+';
    }
</script>

<style>
    .container {
        width: 100%;
        height: 95%;
        max-height: 100%;
        max-width: 800px;
        box-sizing: border-box;
        overflow: hidden;
        display: flex;
        flex-direction: column;
        align-items: stretch;
        justify-content: space-between;
        background: #FFF;
        padding: 10px;
    }

    .todo-list {
        flex: 1;
        margin: 10px 0;
        overflow-y: auto;
        overflow-x: hidden;
    }

    .inner-container {
        display: flex;
        flex-wrap: wrap;
        align-items: center;
        justify-content: space-between;
        padding: 5px 0;
    }

    /* button base */
    button {
        background: #CCC;
        cursor: pointer;
        font-size: 1rem;
        border-radius: 4px;
        border: none;
        outline: none;
        padding: 8px 14px;
        transition: all ease .5s;
    }
    button:hover {
        background: #EEE;
    }
    /* header */
    header {
        display: flex;
        justify-content: start;
        align-items: stretch;
        padding: 20px 0;
    }
    header > .header-left {
        flex: 1;
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: stretch;
    }
    header > .header-left h2 {
        color: #676767;
        font-size: 3rem;
    }
    header > .header-left h3 {
        color: #5a73e0;
        font-size: 1.5rem;
        margin: 6px 0;
    }
    header > .header-right {
        display: flex;
        flex-direction: row;
        justify-content: end;
        align-items: center;
    }

    /* add item circle button  */
    .circle-button-container {
        position: relative;
        height: 100%;
    }
    .circle-button-container .circle-button {
        position: absolute;
        z-index: 100;
        right: 0;
        top: 0;
        color: #FFF;
        background: #5a73e0;
        border-radius: 50%;
        width: 64px;
        height: 64px;
        display: flex;
        align-items: center;
        justify-content: center;
        font-size: 2rem;
        cursor: pointer;
    }
    .circle-button-container .circle-button-drop {
        position: absolute;
        z-index: 50;
        left: -270px;
        width: calc(270px - 32px);
        border-radius: 32px 0 0 32px;
        height: 64px;
        background: #CCC;
        top: 0;
        display: flex;
        align-items: center;
        justify-content: start;
        box-sizing: border-box;
        padding-left: 16px;
        padding-right: 38px;
    }
    .circle-button-container .circle-button-drop input[type="text"] {
        flex: 1;
        width: 100%;
        border: none;
        outline: none;
        background: transparent;
        font-size: 1.2rem;
    }

    /* custom styled checkbox */
    .checkbox {
        display: flex;
        align-items: center;
        justify-content: start;
    }
    .checkbox input[type="checkbox"] {
        position: absolute;
        opacity: 0;
        pointer-events: none;
    }
    .checkbox span {
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
    .checkbox input[type="checkbox"] + span::before {
        content: "";
        position: absolute;
        top: 0;
        left: 0;
        width: 18px;
        height: 18px;
        border: 2px solid#676767;
        border-radius: 5px;
        margin-top: 2px;
        transition: .3s ease;
    }
    .checkbox input[type="checkbox"]:checked + span::before {
        border-color: #5a73e0;
        border-width: 4px;
        border-left-color: transparent;
        border-top-color: transparent;
        border-radius: 0;
        width: 10px;
        margin-top: -6px;
        transform: rotate(40deg);
    }
    /* filter buttons */
    .filter-buttons button{
        background: transparent;
    }
    .filter-buttons button.active {
        background: #5a73e0;
        color: #FFF;
    }

    /*
     * Mobile visualization
     */
    @media screen and (max-width: 600px) {
        .container {
            height: 100%;
            padding: 20px;
            border-radius: 0;
        }

        .inner-container > div{
            width: 100%;
            display: flex;
            align-items: center;
            justify-content: center;
            margin: 2px 0;
        }

        header > .header-left h2 {
            color: #676767;
            font-size: 2rem;
        }
        header > .header-left h3 {
            color: #5a73e0;
            font-size: 1.2rem;
            margin: 6px 0;
        }
    }
</style>

<div class="container">
    <header>
        <div class="header-left">
            <h2>Tarefas</h2>
            <h3>{todosRemaining} tarefas restantes</h3>
        </div>
        <div class="header-right">
            <div class="circle-button-container">
                <div class="circle-button" on:click={handleAddClickButton}>{addingNewItemChar}</div>
                {#if addingNewItem}
                    <div class="circle-button-drop" transition:fly={{ x: 50 }}>
                        <input type="text" class="todo-add-input" placeholder="Adicionar..." bind:value={newTodoTitle} on:keydown={addTodo} />
                    </div>
                {/if}
            </div>
        </div>
    </header>
    
    <div class="todo-list">
        {#each filteredTodos as todo}
            <div class="todo-item-container">
                <TodoItem {...todo} on:deleteTodo={handleDeleteTodo} on:toggleComplete={handleToggleComplete} />
            </div>
        {/each}
    </div>

    <div class="inner-container">
        <label class="checkbox">
            <input class="inner-container-input" type="checkbox" on:change={checkAllTodos} />
            <span>Marcar todos</span>
        </label>
    </div>

    <div class="inner-container">
        <div class="filter-buttons">
            <button on:click={() => updateFilter('all')} class:active="{currentFilter === 'all'}">Todos</button>
            <button on:click={() => updateFilter('active')} class:active="{currentFilter === 'active'}">Ativos</button>
            <button on:click={() => updateFilter('completed')} class:active="{currentFilter === 'completed'}">Completos</button>
        </div>
        <div>
            <button on:click={clearCompleted}>Remover Completos</button>
        </div>
    </div>
</div>