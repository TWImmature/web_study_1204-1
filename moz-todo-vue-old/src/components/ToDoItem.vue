<template>
  <div class="custom-checkbox">
    <input
        type="checkbox"
        :id="id"
        v-model="isDone"
        class="checkbox"
        @change="emitCheckboxChange"
    />
    <label :for="id" class="checkbox-label" @dblclick="requestEdit">{{ label }}</label>
    <button @click="requestEdit" class="edit-button">Edit</button>
  </div>
</template>

<script>
export default {
  props: {
    label: { required: true, type: String },
    done: { default: false, type: Boolean },
    id: { required: true, type: String },
  },
  data() {
    return {
      isDone: this.done,
    };
  },
  methods: {
    emitCheckboxChange() {
      this.$emit('checkbox-changed', this.id);
    },
    requestEdit() {
      this.$emit('edit-requested', this.id);
    },
  },
  watch: {
    isDone(newValue) {
      // 当 isDone 发生变化时，同步更新父组件中的 done 状态
      this.$emit('update:done', newValue);
    },
    done(newValue) {
      // 当父组件中的 done 状态发生变化时，同步更新本组件的 isDone 状态
      this.isDone = newValue;
    },
  },
};
</script>

<style scoped>
.custom-checkbox {
  position: relative;
  display: flex;
  align-items: center;
  min-height: 40px;
  margin-bottom: 10px;
  padding-left: 40px;
  clear: left;
}

.checkbox {
  position: absolute;
  z-index: 1;
  top: -2px;
  left: -2px;
  width: 44px;
  height: 44px;
  margin: 0;
  opacity: 0;
  cursor: pointer;
}

.checkbox-label {
  font-family: Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  font-weight: 400;
  font-size: 16px;
  line-height: 1.25;
  display: inline-block;
  margin-bottom: 0;
  padding: 8px 15px 5px;
  cursor: pointer;
  touch-action: manipulation;
}

.checkbox-label::before {
  content: "";
  box-sizing: border-box;
  position: absolute;
  top: 0;
  left: 0;
  width: 40px;
  height: 40px;
  border: 2px solid currentcolor;
  background: transparent;
}

.checkbox:focus + .checkbox-label::before {
  border-width: 4px;
  outline: 3px dashed #228bec;
}

.checkbox-label::after {
  content: "";
  position: absolute;
  top: 11px;
  left: 9px;
  width: 18px;
  height: 7px;
  transform: rotate(-45deg);
  border: solid;
  border-width: 0 0 5px 5px;
  border-top-color: transparent;
  opacity: 0;
  background: transparent;
}

.checkbox:checked + .checkbox-label::after {
  opacity: 1;
}

.edit-button {
  margin-left: auto;
  padding: 5px 10px;
  border: none;
  background-color: #ccc;
  cursor: pointer;
}

.edit-button:hover {
  background-color: #bbb;
}

@media only screen and (min-width: 40rem) {
  .custom-checkbox,
  .checkbox-label {
    font-size: 19px;
    line-height: 1.31579;
  }
}
</style>
