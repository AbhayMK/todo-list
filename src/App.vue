<template>
  <div>
    <h2>Todo List</h2>
    <input
      v-model="newTodoText"
      placeholder="Add a new todo"
      @keydown.enter="addTodo"
    />
    <button @click="addTodo">Add</button>

    <table>
      <thead>
        <tr>
          <th>Todo</th>
          <th>Status</th>
          <th>Completion Date</th>
          <th>Check Status</th>
          <th>Action</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="item in todoItems" :key="item.id">
          <td>
            <span :class="{ completed: item.status === 'Completed' }">{{
              item.text
            }}</span>
          </td>
          <td>
            <select v-model="item.status">
              <option value="Yet to Start">Yet to Start</option>
              <option value="Ongoing">Ongoing</option>
              <option value="Completed">Completed</option>
            </select>
          </td>
          <td>
            <input type="date" v-model="item.completionDate" />
          </td>
          <td>
            <input
              type="checkbox"
              v-model="item.completed"
              @change="toggleTodoCompletion(item)"
            />
          </td>
          <td>
            <button @click="deleteTodoItem(item.id)">Delete</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  data() {
    return {
      todoItems: [],
      newTodoText: "",
    };
  },
  methods: {
    addTodo() {
      if (this.newTodoText.trim().length === 0) {
        return;
      }

      const newItem = {
        id: this.todoItems.length + 1,
        text: this.newTodoText,
        status: "Yet to Start",
        completionDate: "",
        completed: false,
      };

      this.todoItems.push(newItem);
      this.newTodoText = "";
    },
    deleteTodoItem(id) {
      this.todoItems = this.todoItems.filter((item) => item.id !== id);
    },
    toggleTodoCompletion(item) {
      item.completed = !item.completed;
      this.updateStatus(item);
    },
    updateStatus(item) {
      if (item.completed) {
        item.status = "Completed"; // Set status as "Completed" if checkbox is checked
      } else {
        item.status = "Yet to Start"; // Set status as "Yet to Start" if checkbox is unchecked
      }
    },
  },
};
</script>

<style>
h2 {
  align-items: center;
  color: crimson;
}
table {
  align-items: center;
  width: 100%;
  border-collapse: collapse;
}

th,
td {
  padding: 8px;
  border: 3px solid blueviolet;
  text-align: left;
  border-bottom: 1px solid #ddd;
}

.completed {
  text-decoration: line-through;
}

input[type="checkbox"] {
  margin: 0;
}
</style>
