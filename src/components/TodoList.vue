<template>
  <div class="card mt-2"
    v-for="(todo, index) in todos"
    :key="todo.id"
  >
    <div class="card-body p-2 d-flex align-items-center">
      <div class="form-check flex-fill mr-2">
        <input 
          :id="todo.id" 
          type="checkbox" 
          class="form-check-input"
          :checked="todo.completed"
          @change="toggleComplete(index)"
        >
        <label 
          :for="todo.id" 
          class="form-check-label"
          :class="{'completed' : todo.completed}"
        >{{todo.subject}}</label>
      </div>
      <button 
        class="btn btn-danger btn-sm"
        @click="deleteTodo(index)"
      >DELETE</button>
    </div>
  </div>
</template>

<script>
export default {
  props:{
    todos:{
      type:Array,
      required:true
    }
  },
  emits:['delete-todo','toggle-complete'],
  setup(props, {emit}){
    const deleteTodo = (index) => {
      emit('delete-todo',index)
    }
    const toggleComplete = (index) => {
      emit('toggle-complete',index)
    }
    return{
      deleteTodo,
      toggleComplete
    }
  }
}
</script>

<style scoped>
  .completed{
    color:gray;
    text-decoration: line-through;
  }
</style>