<template>
  <nav>
    <div>
      <img @click="$emit('close')" src="@/assets/close-black.svg" />
      <h2>{{ name }}</h2>
    </div>

    <img @click="$emit('add-board')" src="@/assets/check-black.svg" />
  </nav>
</template>

<script>
export default {
  name: 'Board-Create-Navbar',
  props: {
    name: {
      type: String,
      required: false,
      default: 'Create Board',
    },
  },
};
</script>

<style lang="scss" scoped>
nav {
  width: 100%;
  height: 3rem;
  background: #eee;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0 0.5rem;

  div {
    display: flex;
    justify-content: space-evenly;
    align-items: center;
    img {
      margin-top: 1px;
      width: 30px;
      height: auto;
      cursor: pointer;
      // transform: scale(0.6);
    }
    h2 {
      font-weight: 200;
      margin-left: 0.1rem;
    }
  }

  img {
    margin-top: 1px;
    width: 30px;
    height: auto;
    cursor: pointer;
  }
}
</style>
