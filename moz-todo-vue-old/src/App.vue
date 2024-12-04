<template>
  <div id="app">
    <h1>My To-Do List</h1>
    <to-do-form @todo-added="addToDo"></to-do-form>
    <h2 id="list-summary">{{ listSummary }}</h2>
    <ul aria-labelledby="list-summary" class="stack-large">
      <li v-for="item in ToDoItems" :key="item.id">
        <to-do-item
            :label="item.label"
            :done="item.done"
            :id="item.id"
            @checkbox-changed="updateTodoStatus(item.id)"
            @edit-requested="editToDo(item.id)"
        ></to-do-item>
        <to-do-item-edit-form
            v-if="itemBeingEdited === item.id"
            :label="item.label"
            :id="item.id"
            @item-edited="itemEdited(item.id, $event)"
            @edit-cancelled="cancelEdit"
        ></to-do-item-edit-form>
      </li>
    </ul>
  </div>
</template>

<script>
import ToDoItem from "./components/ToDoItem.vue";
import ToDoForm from "./components/ToDoForm.vue";
import ToDoItemEditForm from "./components/ToDoItemEditForm.vue";
import uniqueId from "lodash.uniqueid";

export default {
  name: "app",
  components: {
    ToDoItem,
    ToDoForm,
    ToDoItemEditForm
  },
  data() {
    return {
      ToDoItems: [
        {id: uniqueId("todo-"), label: "Learn Vue", done: false},
        {id: uniqueId("todo-"), label: "Create a Vue project with the CLI", done: true},
        {id: uniqueId("todo-"), label: "Have fun", done: true},
        {id: uniqueId("todo-"), label: "Create a to-do list", done: false},
      ],
      itemBeingEdited: null,
    };
  },
  methods: {
    addToDo(toDoLabel) {
      this.ToDoItems.push({
        id: uniqueId("todo-"),
        label: toDoLabel,
        done: false,
      });
    },
    updateTodoStatus(todoId) {
      const todoItem = this.ToDoItems.find(item => item.id === todoId);
      if (todoItem) {
        todoItem.done = !todoItem.done;
      }
    },
    editToDo(todoId) {
      this.itemBeingEdited = todoId;
    },
    itemEdited(todoId, newLabel) {
      const todoItem = this.ToDoItems.find(item => item.id === todoId);
      if (todoItem) {
        todoItem.label = newLabel;
      }
      this.cancelEdit();
    },
    cancelEdit() {
      this.itemBeingEdited = null;
    },
  },
  computed: {
    listSummary() {
      const numberFinishedItems = this.ToDoItems.filter((item) => item.done).length;
      return `${numberFinishedItems} out of${this.ToDoItems.length} items completed`;
    },
  },
};
</script>

<style>
/* Global styles */
#app {
  max-width: 100%;
  margin: 0 auto;
  padding: 2rem;
  font-family: Arial, sans-serif;
}

#app h1 {
  display: block;
  min-width: 100%;
  width: 100%;
  text-align: center;
  margin: 0;
  margin-bottom: 1rem;
}

ul {
  list-style: none;
  padding: 0;
}

li {
  margin-bottom: 0.5rem;
}

/* Component-specific styles */
.to-do-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0.5rem;
  border: 1px solid #ccc;
  border-radius: 4px;
}

.to-do-item input[type="checkbox"] {
  margin-right: 1rem;
}

.to-do-form {
  margin-bottom: 1rem;
}

.to-do-form input[type="text"] {
  width: 100%;
  padding: 0.5rem;
  border: 1px solid #ccc;
  border-radius: 4px;
}

.to-do-form input[type="submit"] {
  padding: 0.5rem 1rem;
  border: none;
  border-radius: 4px;
  background-color: #007bff;
  color: white;
  cursor: pointer;
}

.to-do-form input[type="submit"]:hover {
  background-color: #0056b3;
}
</style>
