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
        margin-bottom: 15px;
        display: flex;
        align-items: center;
        justify-content: space-between;
        animation-duration: 0.3s;
        //border: 1px solid white;
        padding: 0 15px;
        box-shadow: 0 2px 5px 0 rgba(0,0,0,.16), 0 2px 10px 0 rgba(0,0,0,.12);
        border-radius: .25rem;
        color: white;
        background: linear-gradient(40deg,#ffd86f,#fc6262)!important;
    }
    .remove-item {
        cursor: pointer;
        margin: 12px 0;
    }
    .remove-item:hover {
        color: lightseagreen; 
    }
    .todo-item-left {
        display: flex;
        align-items: center;
    }
    .todo-item-left > input {
        margin: 0;
    }
    .todo-item-label {
        margin: 12px 0;
        padding-left: 5px;
    }
    .completed {
        text-decoration: line-through;
        color: grey;
    }
</style>

<div class="todo-item"  transition:fly="{{ y: 20, duration: 300 }}">
    <div class="todo-item-left">
        <input type="checkbox" bind:checked={completed} on:change={toggleComplete}>
        <div class="todo-item-label" class:completed={completed}>{title}</div>
    </div>
    <div class="remove-item" on:click={deleteTodo}>
        x
    </div>
</div>