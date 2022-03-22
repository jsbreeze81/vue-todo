<template>
  <div 
    class="card mt-2"
    v-for="(todo, index) in todos"
    :key="todo.id"
  >
    <div class="card-body p-2 d-flex align-items-center">
      <div class="form-check">
        <input 
          class="form-check-input"
          type="checkbox"
          :value="todo.completed"
          :id="todo.id"
          @change="toggleComplete(index)"
        >
        <label 
          class="form-check-label"
          :class="{completed : todo.completed}"
          :for="todo.id"
        >
          {{todo.subject}}
        </label>
      </div>
      <button
        class="btn btn-danger btn-sm ms-auto"
        @click="deleteTodo(index)"
      >
        Delete
      </button>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    todos:{
      type:Array,
      required:true
    }
  },
  emits:['toggle-todo','delete-todo'],
  setup(props, {emit}){
    const deleteTodo = (index) => {
      emit('delete-todo',index);
    }
    const toggleComplete = (index) => {
      emit('toggle-todo',index);
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