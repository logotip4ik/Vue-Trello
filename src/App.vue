<template>
  <div class="main">
    <VNavbar
      @create-board="creatingBoard = !creatingBoard"
      @show-settings="settingsBoards = !settingsBoards"
    ></VNavbar>
    <div class="boards">
      <VBoard
        v-for="board in boards"
        :key="board.name"
        @add-item="addItem($event, board.list)"
        @change-name="changeName($event, board.list)"
        @del-item="delItem($event, board.list)"
        :list="getList(board)"
        :name="board.name"
      ></VBoard>
    </div>
    <VSettings
      :show="settingsBoards"
      :boards="boards"
      @close="settingsBoards = false"
      @save-boards="saveBoards"
    ></VSettings>
    <VBoardCreate
      @add-board="addBoard"
      :show="creatingBoard"
      @close="creatingBoard = !creatingBoard"
    ></VBoardCreate>
  </div>
</template>

<script>
import { v4 } from 'uuid';
import Dexie from 'dexie';

import VBoard from './components/V-Board.vue';
import VNavbar from './components/V-Navbar.vue';
import VSettings from './components/V-Settings.vue';
import VBoardCreate from './components/V-Board-Create.vue';

export default {
  name: 'App',
  data: () => ({
    boards: {
      todo: {
        id: v4(),
        name: 'todo',
        list: 'todoList',
      },
    },
    lists: {
      todoList: [{ name: 'hello', id: v4() }],
    },
    db: {},
    creatingBoard: false,
    settingsBoards: false,
  }),
  mounted() {
    const db = new Dexie('vue-trello');

    db.version(1).stores({
      boards: '&id, name, *items',
    });

    this.db = db;
  },
  methods: {
    addBoard(name) {
      const list = `${name}List`;
      this.boards[name] = {
        id: v4(),
        name,
        list,
      };
      this.lists[list] = [];
    },
    saveBoards(boards) {
      this.boards = boards;
    },
    getList(board) {
      const listName = board.list;

      return this.lists[listName];
    },
    addItem(name, arr) {
      const item = { name, id: v4() };
      this.lists[arr].push(item);
    },
    changeName({ name, idx }, arr) {
      this.lists[arr][idx].name = name;
    },
    delItem(idx, arr) {
      this.lists[arr].splice(idx, 1);
    },
  },
  components: {
    VBoard,
    VNavbar,
    VSettings,
    VBoardCreate,
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
}
.boards {
  height: calc(100% - 3rem);
  padding: 0 0.25rem;
  display: flex;
  justify-content: space-evenly;
  align-items: center;
  flex-wrap: wrap;
}
</style>
