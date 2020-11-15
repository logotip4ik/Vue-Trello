<template>
  <div>
    <h1>{{ name }}</h1>
    <hr />
    <draggable @change="update" @start="drag = true" @end="drag = false" :list="list" group="items">
      <transition-group name="flip-list" mode="out-in" type="transition">
        <VBoardItem
          @del-item="$emit('del-item', idx)"
          v-for="(item, idx) in list"
          :key="item.id"
          :item="item"
        ></VBoardItem>
      </transition-group>
    </draggable>
    <footer>
      <!-- <div @click="showInput" class="plus"></div> -->
      <img @click="showInput" src="@/assets/add-black.svg" class="plus" />
      <transition mode="out-in" name="fade">
        <input ref="inputFocus" @keypress.enter="addItem" v-if="adding" v-model="text" />
      </transition>
    </footer>
  </div>
</template>

<script>
import draggable from 'vuedraggable';

import VBoardItem from './V-Board-Item.vue';

export default {
  name: 'Board',
  data: () => ({
    drag: false,
    adding: false,
    text: '',
  }),
  methods: {
    update() {
      this.$forceUpdate();
    },
    showInput() {
      this.adding = !this.adding;
      setTimeout(() => (this.adding ? this.$refs.inputFocus.focus() : null), 600);
    },
    addItem() {
      this.$emit('add-item', this.text);
      this.adding = false;
      this.text = '';
    },
  },
  props: {
    list: {
      type: Array,
      required: true,
      default: () => [],
    },
    name: {
      type: String,
      required: false,
      default: 'Name',
    },
  },
  components: {
    draggable,
    VBoardItem,
  },
};
</script>

<style lang="scss" scoped>
div {
  box-shadow: 0 0 10px rgba($color: #fff, $alpha: 0.2);
  padding: 0.5rem 0.75rem;
  border-radius: 0.25rem;
  background: #eee;
  display: flex;
  flex-basis: 350px;
  flex-shrink: 10%;
  flex-direction: column;
  max-width: 500px;
  overflow: auto;
  margin: 0.5rem 0.25rem;

  h1 {
    font-weight: 200;
  }

  hr {
    border: none;
    outline: none;
    background: #18181e;
    height: 1px;
    width: 100%;
    margin: 0.5rem 0;
  }
  footer {
    width: 100%;
    display: flex;
    justify-content: space-between;

    input {
      margin-left: 0.5rem;
      flex-basis: 70%;
      appearance: none;
      border-radius: 0.2rem;
      border: 1px solid rgba(#000000, 0.2);
      outline: none;
      box-shadow: 0 0 10px rgba(#000000, 0.1), 0 1px 1px rgba(#000000, 0.2);
      font-size: 16px;
      padding: 0.25rem;
    }

    .plus {
      width: 30px;
      height: auto;
      cursor: pointer;
    }
  }
}
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s;
}
.fade-enter,
.fade-leave-to {
  opacity: 0;
}
.flip-list-move {
  transition: transform 0.5s;
}
</style>
