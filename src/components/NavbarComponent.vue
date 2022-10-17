<template>
  <nav class="navbar bg-light">
    <div class="container">
      <a class="navbar-brand" href="#">
        <img src="../assets/logcurrency.png" alt="logcurrency" width="500" height="100">
      </a>
    </div>
  </nav>
</template>

<script>
export default {
  name: 'NavbarComponent'
}
</script>

<style>
  .navbar {
    background-color: white !important;
  }

  .container {
    max-width: 100%;
  }

  .container > a > img {
    width: 30%;
    height: 30%;
  }
</style>