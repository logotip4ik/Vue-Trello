<template>
  <div class="item">
    <div class="one-line">
      <h4>{{ item.name }}</h4>
      <div @click="$emit('del-item')" class="minus"></div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Board-Item',
  props: {
    item: {
      type: Object,
      required: true,
      default: () => ({}),
    },
  },
};
</script>

<style lang="scss" scoped>
div.item {
  border-radius: 0.5rem;
  box-shadow: 0 0 10px rgba($color: #000000, $alpha: 0.1),
    0 1px 2px rgba($color: #000000, $alpha: 0.2) !important;
  margin-bottom: 0.5rem;

  h4 {
    font-weight: 200;
    display: contents;
  }

  div.minus {
    width: 15px;
    height: 2px;
    border-radius: 10px;
    background: #be2b2b;
    cursor: pointer;
  }

  .one-line {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  &::after {
    margin-top: 5px;
    content: '';
    width: 100%;
    height: 1px;
    background-color: rgba(#000000, 0.3);
  }
}
</style>
