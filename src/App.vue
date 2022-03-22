<template>
  <div class="container">
    <h2>Todo List</h2>
    
    <TodoSimpleFormVue
      @add-todo="addTodo"
    />

    <TodoList
      :todos="todos"
      @delete-todo="deleteTodo"
      @toggle-todo="toggleTodo"
    />

    <hr>
    <div class="d-flex justify-content-center">
      <nav aria-label="Page navigation example mx-auto">
        <ul class="pagination">
          <li class="page-item">
            <a 
              class="page-link" 
              @click="getTodo(currentPage-1)"
              :disabled="currentPage != 1"
            >Previous</a>
          </li>
          <li 
            v-for="page in numberOfPages"
            :key="page"
            class="page-item"
            :class="{active : page == currentPage}"
          >
            <a class="page-link" @click="getTodo(page)">{{page}}</a>
          </li>
          <li class="page-item">
            <button class="page-link" 
              @click="getTodo(currentPage+1)"
              :disabled="currentPage != numberOfPages"
            >Next</button>
          </li>
        </ul>
      </nav>
    </div>
  </div>
</template>

<script>
import { ref, computed } from 'vue';
import TodoSimpleFormVue from './components/TodoSimpleForm.vue';
import TodoList from './components/TodoList.vue';
import axios from 'axios';

export default {
  components:{
    TodoSimpleFormVue,
    TodoList
  },
  setup(){
    const todos = ref([]);
    const numberOfTodos = ref(0);
    const limit = 5;
    const currentPage = ref(1);

    const numberOfPages = computed(() => {
      return Math.ceil(numberOfTodos.value/limit);
    })

    const getTodo = async (page = currentPage.value) => {
      currentPage.value = page;
      try{
        const res = await axios.get(`http://localhost:3000/todos?_page=${page}&_limit=${limit}`);
        numberOfTodos.value = res.headers['x-total-count'];
        todos.value = res.data;
      }catch(err){
        console.log('error');
      }
    }

    getTodo();

    const addTodo = async (todo) => {
      // db에 저장
      try{
        const res = await axios.post('http://localhost:3000/todos',{
          subject: todo.subject,
          completed: todo.completed
        })
        getTodo(1);
        todos.value.push(res.data)
      }catch(err){
        console.log('error');
      }
    }

    const deleteTodo = async (index) => {
      // todos.value.splice(index,1);
      const id = todos.value[index].id;
      try{
        await axios.delete('http://localhost:3000/todos/'+id);
        todos.value.splice(index,1)
      }catch(err){
        console.log('error');
      }
    }

    const toggleTodo = async (index) => {
      // todos.value[index].completed = !todos.value[index].completed
      const id = todos.value[index].id;
      try{
        await axios.patch('http://localhost:3000/todos/'+id,{
          completed:!todos.value[index].completed
        });
        todos.value[index].completed = !todos.value[index].completed;
      }catch(err){
        console.log('error');
      }
    }

    return{
      todos,
      numberOfPages,
      currentPage,
      addTodo,
      deleteTodo,
      toggleTodo,
      getTodo
    }
  }
}
</script>

<style scoped>
</style>