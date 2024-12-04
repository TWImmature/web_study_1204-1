<template>
  <form @submit.prevent="onSubmit">
    <h2 class="label-wrapper">
      <label for="new-todo-input" class="label__lg">
        What needs to be done?
      </label>
    </h2>
    <input
        type="text"
        id="new-todo-input"
        name="new-todo"
        autocomplete="off"
        v-model.lazy.trim="label"
        class="input__lg"
    />
    <button type="submit" class="btn btn__primary btn__lg">Add</button>
  </form>
</template>

<script>
export default {
  data() {
    return {
      label: "",
    };
  },
  methods: {
    onSubmit() {
      if (this.label === "") {
        return;
      }
      this.$emit("todo-added", this.label);
      this.label = "";
    },
  },
};
</script>

<style scoped>
.label-wrapper {
  /* Add styles for label wrapper if needed */
}

.label__lg {
  /* Add styles for large label if needed */
}

.input__lg {
  /* Add styles for large input if needed */
}

.btn {
  /* Add styles for button if needed */
}

.btn__primary {
  /* Add styles for primary button if needed */
}

.btn__lg {
  /* Add styles for large button if needed */
}
</style>
