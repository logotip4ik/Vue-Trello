<template>
  <div class="item">
    <div class="one-line">
      <div>
        <img v-if="!editing" src="@/assets/handle-black.svg" class="handle" />
        <transition @enter="changeName" mode="out-in">
          <h4 @click="showEdit" v-if="!editing">{{ item.name }}</h4>
          <div class="form" v-else>
            <input ref="inputName" v-model="newName" @keypress.enter="save" />
            <img src="@/assets/close-black.svg" @click="cancelEditing" />
          </div>
        </transition>
      </div>
      <img @click="$emit('del-item')" src="@/assets/remove-red.svg" class="minus" />
    </div>
  </div>
</template>

<script>
import { ref } from 'vue';

export default {
  name: 'Board-Item',
  setup(props, { emit }) {
    const inputName = ref(null);

    const editing = ref(false);
    const newName = ref('');

    function save() {
      editing.value = false;
      emit('change-name', newName.value);
    }
    function showEdit() {
      editing.value = !editing.value;
      setTimeout(() => inputName.value.focus(), 100);
    }
    function cancelEditing() {
      editing.value = false;
      newName.value = '';
    }
    function changeName() {
      newName.value = props.item.name;
    }

    return {
      inputName,
      // data
      editing,
      newName,
      // functions
      save,
      showEdit,
      cancelEditing,
      changeName,
    };
  },
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
  max-height: 50px;

  h4 {
    font-size: 1rem;
    font-weight: 200;
    display: contents;
  }

  .minus {
    width: 25px;
    height: auto;
    cursor: pointer;
  }

  .one-line {
    display: flex;
    justify-content: space-between;
    align-items: center;

    .handle {
      width: 22px;
      height: auto;
      vertical-align: middle;
      margin-right: 0.25rem;
      cursor: pointer;
    }

    .form {
      position: relative;
      width: 80%;
      height: 1.5rem;
      box-shadow: 0 0 3px rgba($color: #3691ce, $alpha: 0.8);
      border-radius: 0.25rem;

      input {
        margin-top: 0.1rem;
        padding-right: 1rem;
        width: 97%;
        appearance: none;
        background: transparent;
        border: none;
        outline: none;
        font-size: 1rem;
        font-family: inherit;
        font-weight: 200;
      }

      img {
        position: absolute;
        width: 18px;
        height: auto;
        top: 50%;
        right: 3px;
        transform: translateY(-50%);
        cursor: pointer;
      }
    }
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
