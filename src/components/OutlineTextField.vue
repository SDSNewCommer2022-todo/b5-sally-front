<template>
  <div>
    <div class="todo-textfield">
      <div class="todo-textfield__inputfield">
        <div class="todo-textfield__inputfield__input">
          <input
            ref="content"
            class="todo-textfield__inputfield__input--input"
            aria-label="test"
            type="text"
            placeholder="Enter your task"
            :value="content"
            @input="IsInput"
            @blur="outFocus"
            @focus="onFocus"
            @keyup.enter="storeTask"
          />
          <button
            v-if="content && sending"
            class="todo-textfield__inputfield__input--deletebutton"
            type="button"
            @mousedown="deleteCon"
          ></button>
        </div>
      </div>

      <button
        v-if="sending && content"
        class="todo-textfield__sendbutton--texting"
        type="button"
        @mousedown="storeTask"
      ></button>
      <button v-else class="todo-textfield__sendbutton" type="button"></button>
    </div>
  </div>
</template>

<script>


export default {
  name: 'OutlineTextField',
  data() {
    return {
      inputText: '',
      sending: '',
      content: '',
    };
  },
  methods: {
    IsInput(event) {
      const updatedText = event.target.value;
      this.content = updatedText;
    },
    outFocus() {
      this.sending = null;
    },
    onFocus() {
      this.sending = '1';
    },
    deleteCon() {
      this.content = null;

      setTimeout(() => this.$refs.content.focus(), 0);
    },
    storeTask() {
      if (this.content) {
        this.$emit('addTask',this.content)
        this.content = '';
      }
    },
  },
};
</script>

<style lang="scss" scoped>
$font-family: 'Roboto';
$font-color: #ffffff;

.todo-textfield {
  display: flex;
  flex-direction: row;
  justify-content: left;
  align-items: center;

  &__inputfield {
    flex-direction: column;
    align-items: center;
    padding: 0;
    font-size: 0;

    &__input {
      width:84vw;
      height: 48px;
      display: flex;

      &--input {
        border: 0;
        width: 100%;
        border-left-width: 0;
        border-right-width: 0;
        border-top-width: 0;
        border: solid 1px #cccccc;
        border-radius: 5px;
      }

      &--input::placeholder {
        margin-left: 10px;
        display: flex;
        font-family: $font-family;
        font-weight: 400;
        font-size: 16px;
        line-height: 24px;
      }

      &--input:focus {
        border: 0;
        width: 100%;
        outline: none;
        border-left-width: 0;
        border-right-width: 0;
        border-top-width: 0;
        border: solid 1px dodgerblue;
        border-radius: 5px;
      }

      &--deletebutton {
        width: 20px;
        height: 20px;
        margin-top: 14px;
        margin-left: -30px;
        background: url('~/src/assets/deletebutton.svg');
        background-size: 18px 18px;
        border: none;
        cursor: pointer;
      }
    }
  }

  &__sendbutton {
    margin-left:11px;
    width: 24px;
    height: 24px;
    background: url('~/src/assets/textfieldsendbutton.svg');
    border: none 10px;

    &--texting {
      @extend .todo-textfield__sendbutton !optional;
      background: url('~/src/assets/sendbutton_active.svg');
      cursor: pointer;
    }
  }
}
</style>
