<template>
    <div class="container mt-5">
      <div class="card">
        <div class="card-header">
          <h4>Users
            <RouterLink to="/users/create" class="btn btn-primary float-end">Add User</RouterLink>
          </h4>
        </div>
        <div class="card-body">
          <div v-if="loading" class="text-center">Loading...</div>
          <div v-else>
            <table class="table table-striped table-bordered">
              <thead>
                <tr>
                  <th>ID</th>
                  <th>Name</th>
                  <th>Email</th>
                  <th>Created At</th>
                  <th>Action</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="user in users" :key="user.id">
                  <td>{{ user.id }}</td>
                  <td>{{ user.name }}</td>
                  <td>{{ user.email }}</td>
                  <td>{{ new Date(user.created_at).toLocaleDateString() }}</td>
                  <td>
                    <RouterLink :to="`/users/edit/${user.id}`" class="btn btn-success btn-sm">Edit</RouterLink>
                    <button type="button" @click="confirmDelete(user.id)" class="btn btn-danger btn-sm">Delete</button>
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  
  export default {
    name: 'Users',
    data() {
      return {
        users: [],
        loading: true
      };
    },
    mounted() {
      this.fetchUsers();
    },
    methods: {
      async fetchUsers() {
        try {
          this.loading = true;
          const response = await axios.get('http://127.0.0.1:8000/api/users');
          this.users = response.data.data;
          this.loading = false;
        } catch (error) {
          console.error('An error occurred:', error.message);
          this.loading = false;
        }
      },
      confirmDelete(userId) {
        if (confirm('Are you sure you want to delete this user?')) {
          this.deleteUser(userId);
        }
      },
      async deleteUser(userId) {
        try {
          await axios.delete(`http://127.0.0.1:8000/api/users/${userId}`);
          this.users = this.users.filter(user => user.id !== userId);
          alert('User deleted successfully!');
        } catch (error) {
          console.error('An error occurred:', error.message);
        }
      }
    }
  };
  </script>
  
  <style scoped>
  .table {
    margin-bottom: 0;
  }
  </style>
  