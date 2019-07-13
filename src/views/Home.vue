<template>
  <div class="home">
    <img class="title"src="@/assets/title.png" alt="logo">
    <img class="img2"src="@/assets/rickmorty.png" alt="rickandmorty" height="200px">
  </div>
</template>

<script>
export default {
  name: 'home'
}
</script>

<style>
.home{
  display: flex;
  flex-direction: column;
  align-items: center
}
.title{
  max-width: 100%
}
</style>
