<template>
  <nav>
    <h1>Vue Trello</h1>
    <!-- <div class="plus" @click="$emit('create-board')"></div> -->
    <img class="plus" src="@/assets/add-black.svg" @click="$emit('create-board')" />
  </nav>
</template>

<script>
export default {
  name: 'Navbar',
};
</script>

<style lang="scss" scoped>
nav {
  display: flex;
  justify-content: space-between;
  padding: 0 1rem;
  align-items: center;
  background: #eee;
  height: 3rem;

  h1 {
    font-size: 2rem;
    font-weight: 200;
  }

  .plus {
    cursor: pointer;
    width: 30px;
    height: auto;
  }
}
</style>
