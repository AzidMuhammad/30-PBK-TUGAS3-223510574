<template>
    <div class="container">
      <h1 style="text-align: center;">Task Manager</h1>
      <div style="display: flex;">
        <input type="text" v-model="newTask" placeholder="Add new task...">
        <button @click="addTask">Add</button>
      </div>
  
      <div v-for="(task, index) in incompleteTasks" :key="index" class="task" :class="{ 'completed': task.completed }">
        <input type="checkbox" v-model="task.completed" class="checkbox">
        <div v-if="!task.editing" class="task-text">{{ task.title }}</div>
        <div v-else class="edit-mode">
          <input v-model="task.title" class="edit-input">
          <div class="button-group">
            <button class="update-btn" @click="updateTask(index)">Update</button>
            <button class="cancel-btn" @click="cancelTask(index)">Cancel</button>
          </div>
        </div>
        <div v-if="!task.editing" class="button-group">
          <button class="edit-btn" @click="editTask(index)">Edit</button>
          <button class="delete-btn" @click="deleteTask(index)">Delete</button>
        </div>
      </div>
  
      <button class="filter-btn" @click="showIncomplete = !showIncomplete">
        {{ showIncomplete ? 'Show All Tasks' : 'Show Incomplete Tasks Only' }}
      </button>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        tasks: [],
        newTask: '',
        showIncomplete: true,
      };
    },
    methods: {
      addTask() {
        if (this.newTask.trim() !== '') {
          this.tasks.push({ title: this.newTask, completed: false, editing: false });
          this.newTask = '';
        }
      },
      cancelTask(index) {
        this.tasks[index].editing = false;
      },
      editTask(index) {
        this.tasks[index].editing = true;
      },
      updateTask(index) {
        this.tasks[index].editing = false;
      },
      deleteTask(index) {
        this.tasks.splice(index, 1);
      },
    },
    computed: {
      incompleteTasks() {
        if (this.showIncomplete) {
          return this.tasks.filter(task => !task.completed);
        } else {
          return this.tasks;
        }
      }
    }
  };
  </script>
  
  <style scoped>
  .container {
    width: 100%;
    width: 600px;
    padding: 20px;
    border-radius: 10px;
    box-shadow: 0px 0px 10px #18181a;
    background-color: #29292b;
    font-family: 'Courier New', Courier, monospace;
    margin-top: 50px;
  }
  
  input[type="text"] {
    width: calc(100% - 95px);
    padding: 10px;
    border: 1px solid #ccc;
    border-radius: 5px 0 0 5px;
    outline: none;
    font-family: 'Courier New', Courier, monospace;
  }
  
  button {
    padding: 10px 15px;
    margin: 0 5px;
    background-color: #0c0c0f;
    color: whitesmoke;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    outline: none;
  }
  
  button:hover {
    background-color: #38383a;
  }
  
  .task {
    display: flex;
    align-items: center;
    margin: 10px 0;
    padding: 10px;
    border-radius: 5px;
    background-color: whitesmoke;
    color: #0c0c0f;
  }
  
  .task input[type="checkbox"] {
    margin-right: 10px;
    color: #18181a;
  }
  
  .task-text {
    flex: 1;
    cursor: pointer;
    color: #18181a;
    font-weight: bold;
  }
  
  .edit-input {
    flex: 1;
    margin-right: 10px;
  }
  
  .edit-mode {
    display: flex;
    align-items: center;
  }
  
  .button-group {
    display: flex;
    align-items: center;
  }
  
  .edit-btn,
  .delete-btn,
  .cancel-btn {
    margin-left: 5px;
  }
  
  .update-btn {
    margin-right: 5px;
  }
  
  .completed .task-text {
    text-decoration: line-through;
    opacity: 0.6;
  }
  
  .filter-btn {
    margin-top: 20px;
    padding: 10px 15px;
    background-color: #0c0c0f;
    color: white;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    outline: none;
  }
  
  .filter-btn:hover {
    background-color: #38383a;
  }
  
  .edit-input {
    width: calc(100% - 80px);
    margin-right: 10px;
  }
  
  .checkbox {
    -webkit-appearance: none;
    -moz-appearance: none;
    appearance: none;
    width: 15px;
    height: 15px;
    border: 1px solid #000;
    border-radius: 5px;
    margin-right: 5px;
  }
  
  .checkbox:checked {
    background-color: #0c0c0f;
  }
  
  .button-container {
    text-align: center;
    margin-top: 30px;
  }
  
  .button-container button {
    padding: 10px 20px;
    background-color: whitesmoke;
    color: #0c0c0f;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    margin-right: 10px;
  }
  
  .button-container button:last-child {
    margin-right: 0;
  }
  
  .button-container button:hover {
    background-color: #38383a;
  }
  </style>
  