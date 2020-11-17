<template>
  <div class="main">
    <h2>Menu</h2>
    <div class="item" @click="$emit('create-board')">
      <img src="@/assets/add-black.svg" alt="add" />
      <h3>Add Board</h3>
    </div>
    <div class="item" @click="$emit('show-settings')">
      <img src="@/assets/settings-black.svg" alt="settings" />
      <h3>Setting Board</h3>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Menu',
};
</script>

<style lang="scss" scoped>
.main {
  max-width: 400px;
  display: flex;
  flex-direction: column;
  position: fixed;
  top: 0;
  right: 0;
  height: 100%;
  z-index: 100;
  background: #eee;
  box-shadow: -5px 0 10px rgba($color: #000000, $alpha: 0.3);
  padding: 0.5rem;

  h2 {
    font-weight: 200;
    margin-bottom: 1rem;
  }

  .item {
    display: flex;
    padding: 0.5rem 0.75rem;
    margin-bottom: 1rem;
    position: relative;
    cursor: pointer;

    h3 {
      font-weight: 200;
    }

    img {
      width: 20px;
      height: auto;
      margin-right: 0.5rem;
    }

    &::after {
      content: '';
      position: absolute;
      bottom: 0;
      left: 0;
      width: 100%;
      height: 1px;
      background: rgba($color: #18181e, $alpha: 0.5);
    }
  }
}
</style>
