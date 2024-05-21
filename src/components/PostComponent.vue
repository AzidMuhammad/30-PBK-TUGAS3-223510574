<template>
    <div class="post-container">
      <select v-model="selectedUser" @change="fetchPosts">
        <option v-for="user in users" :value="user.id" :key="user.id">{{ user.name }}</option>
      </select>
      <div class="table-container">
        <table class="post-table">
          <thead>
            <tr>
              <th>Judul</th>
              <th>Isi</th>
              <th>Actions</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="post in userPosts" :key="post.id">
              <td v-if="!post.editing"><strong>{{ post.title }}</strong></td>
              <td v-else><input v-model="post.title" /></td>
              <td v-if="!post.editing">{{ post.body }}</td>
              <td v-else><input v-model="post.body" /></td>
              <td>
                <button v-if="!post.editing" @click="editPost(post)">Edit</button>
                <button v-else @click="updatePost(post)">Save</button>
                <button v-if="post.editing" @click="cancelEdit(post)">Cancel</button>
                <button @click="deletePost(post.id)">Delete</button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        users: [],
        selectedUser: null,
        userPosts: [],
      };
    },
    methods: {
      async fetchUsers() {
        const response = await fetch('https://jsonplaceholder.typicode.com/users');
        this.users = await response.json();
      },
      async fetchPosts() {
        const response = await fetch(`https://jsonplaceholder.typicode.com/posts?userId=${this.selectedUser}`);
        this.userPosts = await response.json();
        this.userPosts.forEach(post => {
          post.editing = false;
        });
      },
      editPost(post) {
        post.editing = true;
      },
      cancelEdit(post) {
        post.editing = false;
        this.fetchPosts();
      },
      async updatePost(post) {
        post.editing = false;
        console.log('Updated Post:', post);
      },
      async deletePost(postId) {
        this.userPosts = this.userPosts.filter(post => post.id !== postId);
      }
    },
    mounted() {
      this.fetchUsers();
    },
    watch: {
      selectedUser() {
        this.fetchPosts();
      },
    }
  };
  </script>
  
  <style scoped>
  select {
    padding: 10px;
    border-radius: 5px;
    border: 1px solid #ccc;
    margin-top: 10px;
    margin-bottom: 5px;
  }
  
  ul {
    list-style: none;
    padding: 0;
  }
  
  li {
    padding: 10px 0;
  }
  
  li:last-child {
    border-bottom: none;
  }
  
  .post-container {
    margin-top: 20px;
    text-align: center;
  }
  
  .table-container {
    margin: 0 auto;
    width: 75%;
    margin-bottom: 50px;
  }
  
  .post-table {
    width: 100%;
    border-collapse: collapse;
    margin-top: 20px;
  }
  
  .post-table th, .post-table td {
    border: 1px solid #ccc;
    padding: 8px;
  }
  
  .post-table th {
    background-color: #0c0c0f;
    color: white;
  }
  
  .post-table td {
    text-align: left;
  }
  </style>
  