<script>
    import TodoItem from './TodoItem.svelte';

    let newTodoTitle = '';
   
    $: todos;
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
    let nextId = Math.max(...todos.map(t => t.id)) + 1;
    function addTodo(event) {
        if (event.key === 'Enter') {
            todos = [...todos, 
                {
                    id: nextId,
                    completed: false,
                    title: newTodoTitle
                }
            ];
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
        background-color: #f7f9fc;
        color: #1a2138;
        font-family: Open Sans,sans-serif;
        -webkit-appearance: none;
        transition-duration: .15s;
        transition-property: border,background-color,color,box-shadow;
        transition-timing-function: ease-in;
        border: 1px solid #edf1f7;
    }
    .todo-card {
        padding: 15px;
        box-shadow: 0 2px 5px 0 rgba(0,0,0,.16), 0 2px 10px 0 rgba(0,0,0,.12);
        border-radius: .25rem;
        color: white;
        background: white;
    }
</style>

<div class="container">
    <div class="todo-card">
        <input 
            type="text" 
            class="todo-input" 
            placeholder="input task..." 
            bind:value={newTodoTitle} 
            on:keydown={addTodo} 
        >
        {#each todos as todo}
            <TodoItem {...todo} 
                on:deleteTodo={handleDeleteTodo} 
                on:toggleComplete={handleToggleComplete} />
        {/each}
    </div>
</div>