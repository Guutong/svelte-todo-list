<script>
    import TodoItem from './TodoItem.svelte';

    let newTodoTitle = '';
    let currentFilter = 'all';
    let nextId = 4;
    let todos = [
        {
            id: 1,
            title: 'Task 1',
            completed: false
        },
        {
            id: 2,
            title: 'Task 2',
            completed: false
        },
        {
            id: 3,
            title: 'Task 3',
            completed: false
        }
    ];
    $: todos;
    function addTodo(event) {
        if (event.key === 'Enter') {
            todos = [...todos, {
                id: nextId,
                completed: false,
                title: newTodoTitle
            }];
            nextId = nextId + 1;
            newTodoTitle = '';
        }
    }
    
    function handleDeleteTodo(event) {
        todos = todos.filter(todo => todo.id !== event.detail.id);
    }

    function handleToggleComplete(event) {
        const todoIndex = todos.findIndex(todo => todo.id === event.detail.id);
        const updatedTodo = { ...todos[todoIndex], completed: !todos[todoIndex].completed};
        todos = [
            ...todos.slice(0, todoIndex),
            updatedTodo,
            ...todos.slice(todoIndex+1)
        ];
    }
</script>

<style>
    .container {
        max-width: 800px;
        margin: 10px auto;
    }
    .todo-input {
        width: 100%;
        padding: 10px, 20px;
        font-size: 18px;
        margin-bottom: 20px;
    }
</style>

<div class="container">
    <h2>Svelte Todo</h2>
    <input type="text" class="todo-input" placeholder="Insert todo item ..." bind:value={newTodoTitle} on:keydown={addTodo} >
    {#each todos as todo}
        <div class="todo-item">
            <TodoItem {...todo} on:deleteTodo={handleDeleteTodo} on:toggleComplete={handleToggleComplete} />
        </div>
    {/each}
</div>