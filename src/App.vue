<template>
  <div class="container">
    <h2>Todo List</h2>
    
    <TodoSimpleForm @add-todo="addTodo" />

    <TodoList
      :todos="todos"
      @delete-todo="deleteTodo"
      @toggle-complete="toggleComplete"
    />

    <Pagination
      :currentPage="currentPage"
      :numberOfPages="numberOfPages"
      @get-todo="getTodo"
    />
  </div>
</template>

<script>
import {ref, computed} from 'vue';
import TodoSimpleForm from './components/TodoSimpleForm.vue';
import TodoList from './components/TodoList.vue';
import axios from 'axios';
import Pagination from './components/Pagination.vue';

export default {
  components:{
    TodoSimpleForm,
    TodoList,
    Pagination
  },
  setup(){
    const todos = ref([]);
    const numberOfTodos = ref(0);
    const limit = 5
    const currentPage = ref(1);

    const numberOfPages = computed(() => {
      return Math.ceil(numberOfTodos.value/limit);
    })

    const getTodo = async(page = currentPage.value) => {
      currentPage.value = page;
      try{
        const res = await axios.get(`http://localhost:3000/todos?_sort=id&_order=desc&_page=${page}&_limit=${limit}`)
        todos.value = res.data;
        numberOfTodos.value = res.headers['x-total-count'];
      }catch(err){
        console.log(err);
      }
    }

    getTodo();
    
    const addTodo = async (todo) => {
      try{
        await axios.post('http://localhost:3000/todos',{
          subject:todo.subject,
          completed:todo.completed
        })
        getTodo(1);
      }catch(err){
        console.log(err);
      }
      todos.value.push(todo);
    }

    const deleteTodo = async (index) => {
      const id = todos.value[index].id
      try{
        await axios.delete('http://localhost:3000/todos/'+id);
        todos.value.splice(index,1);
      }catch(err){
        console.log(err);
      }
    }

    const toggleComplete = async (index) => {
      const id = todos.value[index].id;
      try{
        await axios.patch('http://localhost:3000/todos/'+id,{
          completed:!todos.value[index].completed
        });
        todos.value[index].completed = !todos.value[index].completed
      }catch(err){
        console.log(err);
      }
    }

    return{
      todos,
      numberOfPages,
      currentPage,
      addTodo,
      deleteTodo,
      toggleComplete,
      getTodo
    }
  }
}
</script>

<style scoped>
  
</style>