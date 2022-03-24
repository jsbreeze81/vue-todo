<template>
  <form @submit.prevent="addTodo">
    <div class="d-flex">
      <div class="flex-grow-1 me-2">
        <input 
          type="text" 
          class="form-control"
          v-model="todo"
        >
      </div>
      <button class="btn btn-primary" type="submit">ADD</button>
    </div>
  </form>
  <p v-if="isError">내용을 입력해주세요</p>
</template>

<script>
import {ref} from 'vue';
export default {
  emits:['add-todo'],
  setup(props, {emit}){
    const todo = ref('');
    const isError = ref(false);

    const addTodo = () => {
      if(todo.value.trim() === ''){
        isError.value = true;
      }else{
        emit('add-todo',{
          subject:todo.value,
          completed:false
        })
        isError.value = false;
      }
      todo.value = '';
    }

    return{
      todo,
      isError,
      addTodo
    }
  }
}
</script>

<style>

</style>