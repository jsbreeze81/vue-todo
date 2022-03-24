<template>
  <div class="d-flex justify-content-center mt-4">
    <nav aria-label="Page navigation example">
      <ul class="pagination">
        <li class="page-item">
          <a class="page-link" href="#" 
            @click="getTodo(currentPage-1)"
            v-if="currentPage > 1"
          >Previous</a>
        </li>
        <li class="page-item"
          v-for="page in numberOfPages"
          :key="page"
        >
          <a class="page-link" href="#"
            @click="getTodo(page)"
          >{{page}}</a>
        </li>
        <li class="page-item">
          <a 
            class="page-link" href="#" 
            @click="getTodo(currentPage+1)"
            v-if="currentPage < numberOfPages"
          >Next</a>
        </li>
      </ul>
    </nav>
  </div>
</template>

<script>
export default {
  props:{
    numberOfPages:{
      type:Number,
      required:true
    },
    currentPage:{
      type:Number,
      required:true
    }
  },
  setup(props,{emit}){
    const getTodo = (pageNumber) => {
      emit('get-todo',pageNumber)
    }
    return{
      getTodo
    }
  }
}
</script>

<style>

</style>