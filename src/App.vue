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
// import Dexie from 'dexie';

import VBoard from './components/V-Board.vue';
import VNavbar from './components/V-Navbar.vue';
import VSettings from './components/V-Settings.vue';
import VBoardCreate from './components/V-Board-Create.vue';

export default {
  name: 'App',
  setup() {
    const creatingBoard = ref(false);
    const settingsBoards = ref(false);
    const boards = reactive({
      todo: {
        id: v4(),
        name: 'todo',
        items: [
          {
            id: v4(),
            name: 'test',
          },
        ],
      },
    });
    // BOARD Functions
    function addBoard(name) {
      boards[name] = {
        id: v4(),
        name,
        items: [],
      };
    }
    // ITEM Functions
    function addItem(itemName, boardName) {
      const item = { id: v4(), name: itemName };
      boards[boardName].items.push(item);
    }
    function changeName({ name: newName, idx: itemIdx }, board) {
      boards[board].items[itemIdx].name = newName;
    }
    function delItem(itemIdx, board) {
      boards[board].items.splice(itemIdx, 1);
    }
    return {
      // data
      boards,
      creatingBoard,
      settingsBoards,
      // Functions
      addBoard,
      addItem,
      changeName,
      delItem,
    };
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
