<template>
  <form
    @submit.prevent="addTodo"
  >
    <div class="d-flex">
      <div class="flex-grow-1 me-2">
        <input 
          type="text" 
          class="form-control"
          v-model="todo"
        >
      </div>
      <div>
        <button type="submit" class="btn btn-primary">ADD</button>
      </div>
    </div>
    <p 
      class="mt-1 text-danger"
      v-if="emptyInput"
    >
      내용을 입력해주세요
    </p>
  </form>
</template>

<script>
import {ref} from 'vue';

export default {
  emits:['add-todo'],
  setup(props, {emit}){
    const todo = ref('');
    const emptyInput = ref(false);

    const addTodo = () => {
      if(todo.value === ''){
        emptyInput.value = true;
      }else{
        emit('add-todo',{
          id : Date.now(),
          subject : todo.value,
          completed : false
        })
        todo.value = '';
        emptyInput.value = false;
      }
    }

    return{
      todo,
      emptyInput,
      addTodo
    }
  }
}
</script>

<style>

</style>