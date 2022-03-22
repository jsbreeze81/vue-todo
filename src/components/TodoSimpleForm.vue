<template>
  <form
    @submit.prevent="onSubmit"
  >
    <div class="d-flex">
      <div class="flex-grow-1 me-2">
        <input 
          type="text"
          class="form-control"
          v-model="todo"
        >
      </div>
      <button
        type="submit"
        class="btn btn-primary"
      >ADD</button>
    </div>
  </form>
</template>

<script>
import {ref} from 'vue';

export default {
  emits:['add-todo'],
  setup(props, {emit}){

    const todo = ref("");
    const onSubmit = () => {
       emit('add-todo',{
         id:Date.now(),
         subject:todo.value,
         completed:false
       })
      
      todo.value = "";
    };
    
    return{
      todo,
      onSubmit
    }
  }
}
</script>

<style>

</style>