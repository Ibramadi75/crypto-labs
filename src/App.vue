<template>
    <nav>
      <router-link to="/">Home</router-link> |
      <router-link to="/about">About</router-link>
    </nav>
    <router-view/>
</template>

<style>
@import url('https://fonts.googleapis.com/css2?family=Baloo+2:wght@400;500;600;700;800&display=swap');

#app {
  font-family: 'Baloo 2', cursive;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
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

button{
  padding: 10px;
  margin: 10px;
  border: 1px solid rgb(199, 195, 253);
  border-radius: 20px;
  transition-duration: .3s;
  cursor: pointer;
}
button:hover{
  box-shadow: rgba(50, 50, 93, 0.25) 0px 0px 0px -102px inset, rgba(0, 0, 0, 0.3) 0px 8px 26px -8px inset;
}
</style>