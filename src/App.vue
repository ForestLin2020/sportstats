<template>
<header>
  <div id="nav">
    <img class="y-logo" alt="BYU logo" src="https://byucougars.com/themes/Cougar/byu.png">
    <div class="menu-item"><router-link to="/">Statistics</router-link></div>
    <div class="menu-item"><router-link to="/about">About</router-link></div>
  </div>
</header>
<div class="router-view">
  <router-view/>
</div>
</template>
<style>

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

header {
  width: 100%;
  background-color: #0d6efd!important;
}
/* #003DA5 */
/* #0b5ed7 */
/* #0d6efd */

.y-logo{
  width: 45px;
  position: fixed;
  /* top: 5px; */
  left: 5px;
}

#nav {
  display: flex;
  align-items: center;
  justify-content: center;
  position: fixed;
  background-color: #0b5ed7;
  top: 0;
  width: 100%;
  z-index: 10;
}

#nav .menu-item {
  color: #fff;
  padding: 10px 20px 5px 20px;
  position: relative;
  text-align: center;
  border-bottom: 3px solid transparent;
  display: flex;
  transition: 0.4s;
}

#nav .menu-item.active,
#nav .menu-item:hover {
  background-color: #0b5ed7;
  border-bottom-color: #ffffff;
}

#nav .menu-item a{
  color: inherit;
  text-decoration: none;
}

.router-view {
  margin-top: 60px;
}

</style>
