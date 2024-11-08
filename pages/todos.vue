<template>
  <div>
    <h1>ToDos page</h1>
    <NuxtLink to="/">Home</NuxtLink>
    <br />
    <br />
    <!-- Input fields for new ToDo item -->
    <input
      v-model="newToDoDescription"
      type="text"
      placeholder="Enter description"
    />
    <input v-model="newToDoContent" type="text" placeholder="Enter content" />
    <br />
    <!-- Button to add a ToDo item -->
    <button @click="addToDo">Add Todo</button>
    <br />
    <br />
    <br />

    <!-- Only display headings and todos if there are items in the list -->
    <div v-if="todos.length > 0">
      <div class="todo-header">
        <div class="todo-description"><strong>Description</strong></div>
        <div class="todo-content"><strong>Content</strong></div>
        <div class="todo-actions"><strong>Delete</strong></div>
        <div class="todo-actions"><strong>Remove</strong></div>
      </div>

      <!-- Display the list of todos, get both the object and its index -->
      <div v-for="(todo, index) in todos" :key="index" class="todo-item">
        <!-- Row for each todo -->
        <div class="todo-row">
          <div class="todo-description">{{ todo.description }}</div>
          <div class="todo-content">{{ todo.content }}</div>
          <div class="todo-actions">
            <button @click="deleteToDo(index)">Delete</button>
          </div>
          <div class="todo-actions">
            <button @click="removeToDo(todo)">Remove</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts" setup>
// Define the structure of each ToDo item
interface ToDoItem {
  description: string;
  content: string;
}

// Initialize to an empty array of ToDo items
const todos = useState<ToDoItem[]>("todos", () => []);

// Reactive properties to store new ToDo inputs
const newToDoDescription = ref("");
const newToDoContent = ref("");

// Function to add a new ToDo item
function addToDo() {
  // Create a new ToDoItem with the values
  const newToDo: ToDoItem = {
    description: newToDoDescription.value,
    content: newToDoContent.value,
  };

  // Make sure that both description and content have been inputted
  if (newToDoDescription.value > "" && newToDoContent.value > "") {
    // Add the new ToDo item to the array
    todos.value.push(newToDo);
  }
  // Reset input fields after adding
  newToDoDescription.value = "";
  newToDoContent.value = "";
}

// Function to delete a ToDo item by index
const deleteToDo = (index: number) => {
  todos.value.splice(index, 1); // Remove the item at the specified index
};

// This is the solution which passes in the object
// I added it just to show the differences between my project and the provided solution
// I think that passing the object is a better approach, the index will only work
// if the deleteToDo is called from the list whereas removeToDo will work no matter
// where its called from
// whereas the ToDo object/item
const removeToDo = (todo: ToDoItem) => {
  const index = todos.value.indexOf(todo);
  todos.value.splice(index, 1);
};
</script>

<style scoped>
/* Style for individual todo items */
.todo-item {
  margin-bottom: 5px;
}

/* Styling for each header */
.todo-header {
  display: flex;
  align-items: center;
  padding: 8px;
  border: 1px solid #ddd;
  border-radius: 4px;
  margin-bottom: 5px;
}

/* Styling for each row */
.todo-row {
  display: flex;
  align-items: center;
  padding: 8px;
  border: 1px solid #ddd;
  border-radius: 4px;
}

/* Each column's styling */
.todo-description,
.todo-content {
  flex: 1;
  padding: 5px;
  text-align: left;
}

.todo-actions {
  flex: 0 0 80px;
  padding-left: 10px;
}

/* Button styling */
button {
  padding: 8px 15px;
  font-size: 14px;
  cursor: pointer;
  margin-top: 5px;
}
</style>
