<template>
  <div 
    class="card mt-2"
    v-for="(todo, index) in todos"
    :key="todo.id"
  >
    <div class="card-body d-flex align-items-center p-2">
      <div class="form-check">
        <input 
          type="checkbox" 
          class="form-check-input"
          :checked="todo.completed"
          :id="todo.id"
          @change="toggleCheck(index)"
        >
        <label 
          class="form-check-label"
          :class="{completed : todo.completed}"
          :for="todo.id" 
        >
          {{todo.subject}}
        </label>
      </div>
      <div class="ms-auto">
        <button 
          class="btn btn-danger btn-sm"
          @click="deleteTodo(index)"
        >
          DELETE
        </button>
      </div>
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
  emits:['delete-todo','toggle-todo'],
  setup(props, {emit}){
    const deleteTodo = (index) => {
      emit('delete-todo',index)
    }

    const toggleCheck = (index) => {
      emit('toggle-todo',index)
    }

    return{
      deleteTodo,
      toggleCheck
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