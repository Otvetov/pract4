<template>

  <header>

    <nav class="navbar navbar-expand-md  navbar-dark bg-dark">
      <div class="container-fluid">
        <NuxtLink class="navbar-brand" to="/">KinoTower</NuxtLink>
        <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarCollapse" aria-controls="navbarCollapse" aria-expanded="false" aria-label="Toggle navigation">
          <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse justify-content-end" id="navbarCollapse">
          <button @click="$router.push('/signup')"  class="btn btn-outline-success me-3" type="submit">SignUp</button>
          <button class="btn btn-outline-info" type="submit">Sign up</button>
        </div>
      </div>
    </nav>
  </header>


  <main class="flex-shrink-0">
    <div class="container">
      <slot />
    </div>
  </main>

<!--  <footer class="footer mt-auto py-3 fixed-bottom bg-body-tertiary">-->
<!--    <div class="container">-->
<!--      <span class="text-body-secondary">&copy;Kino-Tower-2024</span>-->
<!--    </div>-->
<!--  </footer>-->




</template>
<script setup lang="ts">

</script>