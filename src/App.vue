<template>
  <nav class="navbar navbar-expand-lg navbar-light bg-light">
    <div class="container-fluid">
      <router-link class="navbar-brand" to="/">TODO</router-link>
      <ul class="navbar-nav me-auto flex-row gap-4">
        <li class="nav-item">
          <router-link class="nav-link active" aria-current="page" to="/todos">Todos</router-link>
        </li>
      </ul>
    </div>
  </nav>
  <router-view/>
</template>

<script>
export default {
 
}
</script>

<style scoped>
  
</style>