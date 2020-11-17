<template>
  <transition @enter="enterAnim" @before-leave="leaveAnim" :duration="{ leave: 800 }">
    <div v-if="show" class="bg">
      <VBoardCreateNavbar
        name="Change Boards"
        @close="$emit('close')"
        @add-board="save"
      ></VBoardCreateNavbar>
      <div class="main">
        <div>
          <h1>Boards</h1>
          <hr />
          <draggable @change="update" v-model="list" group="boards" handle=".handle">
            <transition-group name="flip-list" mode="out-in" type="transition">
              <VBoardItem
                @del-item="delItem(item.name)"
                @change-name="changeName($event, item.name)"
                v-for="item in rawBoards"
                :key="item.id"
                :item="item"
              ></VBoardItem>
            </transition-group>
          </draggable>
        </div>
      </div>
    </div>
  </transition>
</template>

<script>
import gsap from 'gsap';

import draggable from 'vuedraggable';
import VBoardItem from './V-Board-Item.vue';
import VBoardCreateNavbar from './V-Board-Create-Navbar.vue';

export default {
  name: 'Settings',
  data: () => ({
    rawBoards: {},
  }),
  mounted() {
    window.addEventListener('keyup', (event) => {
      if (!this.show) return;
      if (event.key === 'Escape') {
        this.$emit('close');
      }
    });
  },
  computed: {
    list: {
      get() {
        return Object.values(this.rawBoards);
      },
      set(val) {
        const boards = {};
        val.forEach((item) => {
          boards[item.name] = item;
        });
        this.rawBoards = boards;
      },
    },
  },
  methods: {
    save() {
      this.$emit('save-boards', this.rawBoards);
      this.$emit('close');
    },
    update() {
      this.$forceUpdate();
    },
    changeName(newName, objName) {
      this.rawBoards[objName].name = newName;
    },
    delItem(name) {
      delete this.rawBoards[name];
      this.$forceUpdate();
    },
    enterAnim(el) {
      this.rawBoards = { ...this.boards };
      gsap.from(el, {
        yPercent: 100,
        ease: 'power2.out',
      });
      this.$forceUpdate();
    },
    leaveAnim(el) {
      gsap.to(el, {
        duration: 0.8,
        yPercent: 100,
        ease: 'power2.out',
      });
    },
  },
  components: {
    draggable,
    VBoardItem,
    VBoardCreateNavbar,
  },
  props: {
    boards: {
      type: Object,
      required: true,
      default: () => ({}),
    },
    show: {
      type: Boolean,
      required: true,
      default: false,
    },
  },
};
</script>

<style lang="scss" scoped>
.bg {
  position: fixed;
  background: #18181e;
  top: 0;
  bottom: 0;
  right: 0;
  left: 0;
  // display: flex;

  .main {
    height: calc(100% - 3rem);
    padding: 0 0.25rem;
    display: flex;
    justify-content: space-evenly;
    align-items: center;
    flex-wrap: wrap;

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
  }
}
.flip-list-move {
  transition: transform 0.5s;
}
</style>
