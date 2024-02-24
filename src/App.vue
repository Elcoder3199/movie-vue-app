<template>
<header class="header">
  <router-link to="/">
    <h1><span>Vue</span>Movies</h1>
  </router-link>
</header>
  <main>
    <router-view/>
  </main>
</template>

<style lang="scss">

.header {
  padding: 10px 10px;
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: #444;
  box-shadow: 0 0 7px rgba(#000, 0.1);
  h1 {
    color: #fff;
    text-transform: uppercase;
    span {
      color: #c6bd0e;
    }
  }
}
</style>
