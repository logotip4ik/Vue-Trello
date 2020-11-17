<template>
  <transition @enter="enterAnim" @before-leave="leaveAnim" :duration="{ leave: 800 }">
    <div class="bg" v-if="show">
      <VBoardCreateNavbar @add-board="save" @close="$emit('close')"></VBoardCreateNavbar>
      <div class="main">
        <h3>Name for new Board:</h3>
        <input @keypress.enter="save" ref="inputName" v-model="name" />
        <div class="buttons">
          <button @click="save" class="save">Save</button>
          <button @click="cancel" class="cancel">Cancel</button>
        </div>
      </div>
    </div>
  </transition>
</template>

<script>
import gsap from 'gsap';

import VBoardCreateNavbar from './V-Board-Create-Navbar.vue';

export default {
  name: 'Craete-Board',
  data: () => ({
    name: '',
  }),
  mounted() {
    window.addEventListener('keyup', (event) => {
      if (!this.show) return;
      if (event.key === 'Escape') {
        this.$emit('close');
      }
    });
  },
  methods: {
    save() {
      if (!this.name) return;
      this.$emit('add-board', this.name);
      this.$emit('close');
      this.name = '';
    },
    cancel() {
      this.name = '';
      this.$emit('close');
    },
    enterAnim(el) {
      gsap.from(el, {
        yPercent: 100,
        ease: 'power2.out',
        onComplete: () => this.$refs.inputName.focus(),
      });
    },
    leaveAnim(el) {
      gsap.to(el, {
        duration: 0.8,
        yPercent: 100,
        ease: 'power2.out',
      });
    },
  },
  props: {
    show: {
      type: Boolean,
      required: true,
      default: false,
    },
  },
  components: {
    VBoardCreateNavbar,
  },
};
</script>

<style lang="scss" scoped>
.bg {
  background: #18181e;
  position: fixed;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  text-align: center;

  .main {
    display: flex;
    flex-direction: column;
    max-width: 400px;
    max-height: 400px;
    padding: 1rem;
    margin: 0.5rem auto;
    border-radius: 0.25rem;
    background: #eee;
    text-align: left;

    h3 {
      font-weight: 200;
      margin-bottom: 1rem;
    }
    input {
      appearance: none;
      border-radius: 0.2rem;
      border: 1px solid rgba(#000000, 0.2);
      outline: none;
      box-shadow: 0 0 10px rgba(#000000, 0.1), 0 1px 1px rgba(#000000, 0.2);
      font-size: 16px;
      padding: 0.25rem;
    }

    .buttons {
      margin-top: auto;
      margin-left: auto;

      * {
        appearance: none;
        border: none;
        outline: none;
        border-radius: 0.25rem;
        box-shadow: 0 0 10px rgba(#000000, 0.1), 0 1px 1px rgba(#000000, 0.2);
        padding: 0.5rem 1.25rem;
        font-size: 1rem;
        transition: transform 100ms ease-out;

        &:hover,
        :focus {
          transform: scale(1.05);
        }
        &:active {
          transform: scale(0.95);
        }
      }

      .save {
        background: #18181e;
        border: 1px solid rgba(#fff, 0.2);
        margin-right: 0.5rem;
        color: whitesmoke;
      }
      .cancel {
        background: #eee;
        border: 1px solid rgba(#000, 0.2);
      }
    }
  }
}
</style>
