<template>
  <div class="main">
    <VBoard
      @add-item="addItem($event, 'todo')"
      @del-item="delItem($event, 'todo')"
      :list="arrays.todo"
      name="ToDo"
    ></VBoard>
    <VBoard
      @add-item="addItem($event, 'done')"
      @del-item="delItem($event, 'done')"
      :list="arrays.done"
      name="Done"
    ></VBoard>
  </div>
</template>

<script>
import VBoard from './components/V-Board.vue';

export default {
  name: 'App',
  data: () => ({
    arrays: {
      todo: [{ name: 'Hello', id: 123 }],
      done: [{ name: 'World', id: 456 }],
    },
  }),
  methods: {
    addItem(name, arr) {
      this.arrays[arr].push({ name, id: Math.floor(Math.random() * 1000) });
    },
    delItem(idx, arr) {
      this.arrays[arr].splice(idx, 1);
    },
  },
  components: {
    VBoard,
  },
};
</script>

<style lang="scss">
*,
::before,
::after {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body,
html {
  width: 100%;
  height: 100%;
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell,
    'Open Sans', 'Helvetica Neue', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  background: #18181e;
}
#app {
  width: 100%;
  height: 100%;
}
.main {
  height: 100%;
  display: flex;
  justify-content: space-evenly;
  align-items: center;
  flex-wrap: wrap;
}
</style>
