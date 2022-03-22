<template>
  <div class="container">
    <h2>Todo List</h2>
    
    <TodoSimpleForm
      @add-todo="addTodo"
    />

    <TodoList
      :todos="todos"
      @delete-todo="deleteTodo"
      @toggle-todo="toggleTodo"
    />
  </div>
</template>

<script>
import {ref} from 'vue';
import TodoSimpleForm from './components/TodoSimpleForm.vue';
import TodoList from './components/TodoList.vue';

export default {
    components: { 
      TodoSimpleForm ,
      TodoList
    },
    setup() {
        const todo = ref("");
        const todos = ref([]);
        const addTodo = (todo) => {
          todos.value.push(todo);
        };
        const deleteTodo = (index) => {
            todos.value.splice(index, 1);
        };
        const toggleTodo = (index) => {
            todos.value[index].completed = !todos.value[index].completed;
        };
        return {
            todo,
            todos,
            addTodo,
            deleteTodo,
            toggleTodo
        };
    },
}
</script>

<style scoped>
  .completed{
    color:gray;
    text-decoration: line-through;
  }
</style>