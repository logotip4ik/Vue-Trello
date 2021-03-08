<template>
  <transition @enter="enterAnim" @before-leave="leaveAnim" :duration="{ enter: 800, leave: 800 }">
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
          <draggable v-model="list" group="boards" handle=".handle">
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
import { computed, onMounted, ref } from 'vue';
import gsap from 'gsap';

import { VueDraggableNext } from 'vue-draggable-next';
import VBoardItem from './V-Board-Item.vue';
import VBoardCreateNavbar from './V-Board-Create-Navbar.vue';

export default {
  name: 'Settings',
  setup(props, { emit }) {
    const rawBoards = ref({});
    const list = computed({
      get: () => Object.values(rawBoards.value),
      set: (val) => {
        const boards = {};
        val.forEach((item) => {
          boards[item.name] = item;
        });
        rawBoards.value = boards;
      },
    });

    onMounted(() => {
      window.addEventListener('keyup', (event) => {
        if (!props.show) return;
        if (event.key === 'Escape') {
          emit('close');
        }
      });
    });

    // ANIMATIONS
    function enterAnim(el) {
      rawBoards.value = props.boards;
      gsap.from(el, {
        yPercent: 100,
        ease: 'power2.out',
      });
    }
    function leaveAnim(el) {
      gsap.to(el, {
        duration: 0.8,
        yPercent: 100,
        ease: 'power2.out',
      });
    }

    function save() {
      emit('save-boards', rawBoards.value);
      emit('close');
    }
    function changeName(newName, objName) {
      rawBoards.value[objName].name = newName;
    }
    function delItem(boardName) {
      delete rawBoards.value[boardName];
    }

    return {
      // data
      list,
      rawBoards,
      // functions
      save,
      changeName,
      delItem,
      enterAnim,
      leaveAnim,
    };
  },
  mounted() {},
  components: {
    draggable: VueDraggableNext,
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
