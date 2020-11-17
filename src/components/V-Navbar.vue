<template>
  <nav>
    <h1>Vue Trello</h1>
    <img
      class="plus"
      ref="image"
      src="@/assets/menu-black.svg"
      @click="showingMenu = !showingMenu"
    />
    <transition @enter="enterAnim" @leave="leaveAnim">
      <VMenu
        v-if="showingMenu"
        @create-board="showCreateBoard"
        @show-settings="showSettingsBoards"
      ></VMenu>
    </transition>
  </nav>
</template>

<script>
import gsap from 'gsap';

import VMenu from './V-Menu.vue';

export default {
  name: 'Navbar',
  data: () => ({
    showingMenu: false,
  }),
  methods: {
    enterAnim(el) {
      gsap.to(this.$refs.image, {
        xPercent: -500,
        zIndex: 1000,
        background: '#eee',
        rotate: 180,
        borderRadius: '0.25rem',
      });
      gsap.from(el, {
        xPercent: 100,
      });
    },
    leaveAnim(el) {
      gsap.to(this.$refs.image, {
        xPercent: 0,
        zIndex: 0,
        rotate: 0,
        duration: 0.3,
      });
      gsap.to(el, {
        xPercent: 100,
      });
    },
    showSettingsBoards() {
      this.$emit('show-settings');
      this.showingMenu = false;
    },
    showCreateBoard() {
      this.$emit('create-board');
      this.showingMenu = false;
    },
  },
  components: {
    VMenu,
  },
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
