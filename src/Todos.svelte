<script>
    import TodoItem from './TodoItem.svelte';

    let newTodoTitle = '';
    let currentFilter = 'all';
    let nextId = 4;

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
</script>

<style>
    .container {
        width: 100%;
        height: 90%;
        max-width: 800px;
        margin: 10px auto;
        display: flex;
        flex-direction: column;
        align-items: stretch;
        justify-content: space-between;
        background: #FFF;
        box-sizing: border-box;
        padding: 20px;
        border-radius: 6px;
        box-shadow: 1px 1px 2px rgba(0, 0, 0, 0.2),
                    3px 3px 8px rgba(0, 0, 0, 0.2);
    }

    .container h2 {
        font-size: 24px;
        text-align: center;
        margin-bottom: 20px;
        font-family: 'Mali', cursive;
    }

    .todo-input {
        font-size: 16px;
        padding: 10px 4px;
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

    button {
        background: #CCC;
        border: none;
        outline: none;
        padding: 6px 10px;
        transition: all ease .5s;
    }

    button:hover {
        cursor: pointer;
        background: #BBB;
    }

    button.active {
        background: #999;
    }

    /* checkall checkbox */
    label.checkbox {
        display: flex;
        align-items: center;
        justify-content: start;
    }
    label.checkbox > input {
        margin-right: 6px;
    }

    @media screen and (max-width: 600px) {
        .container {
            width: 100%;
            height: 100%;
            border-radius: 0;
        }

        .inner-container > div{
            width: 100%;
            display: flex;
            align-items: center;
            justify-content: center;
            margin: 2px 0;
        }
    }
</style>

<div class="container">
    <h2>Svelte To-Do App</h2>
    <input type="text" class="todo-input" placeholder="Inserir item..." bind:value={newTodoTitle} on:keydown={addTodo} />

    <div class="todo-list">
        {#each filteredTodos as todo}
            <div class="todo-item-container">
                <TodoItem {...todo} on:deleteTodo={handleDeleteTodo} on:toggleComplete={handleToggleComplete} />
            </div>
        {/each}
    </div>

    <div class="inner-container">
        <div>
            <label class="checkbox"><input class="inner-container-input" type="checkbox" on:change={checkAllTodos} />Marcar todos</label>
        </div>
        <div>Faltam {todosRemaining} itens</div>
    </div>

    <div class="inner-container">
        <div>
            <button on:click={() => updateFilter('all')} class:active="{currentFilter === 'all'}">Todos</button>
            <button on:click={() => updateFilter('active')} class:active="{currentFilter === 'active'}">Ativos</button>
            <button on:click={() => updateFilter('completed')} class:active="{currentFilter === 'completed'}">Completos</button>
        </div>
        <div>
            <button on:click={clearCompleted}>Remover Completos</button>
        </div>
    </div>
</div>