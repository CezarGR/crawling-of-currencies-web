<template>
  <router-view/>
</template>

<style>
body {
  background-image: url('./assets/background.svg');
  height: 100vh;
  background-position: center;
  background-repeat: no-repeat;
  background-size: cover;
}
#app {
  font-family: 'Kanit', sans-serif;
  text-align: center;
  color: black;
}

nav {
  padding: 30px;
}

nav a {
  font-weight: bold;
  color: #2c3e50;
}

nav a.router-link-exact-active {
  color: #42b983;
}

.color-orange {
  color: #FF8400;
}
</style>
