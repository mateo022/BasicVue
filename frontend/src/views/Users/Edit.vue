<template>
    <div class="container mt-5">
      <div class="card">
        <div class="card-header">
          <h4>Edit User</h4>
        </div>
        <div class="card-body">
          <div v-if="loading" class="text-center">Loading...</div>
          <div v-else>
            <form @submit.prevent="updateUser">
              <div class="mb-3">
                <label for="name" class="form-label">Name</label>
                <input type="text" v-model="model.user.name" class="form-control" id="name" required>
              </div>
              <div class="mb-3">
                <label for="email" class="form-label">Email</label>
                <input type="email" v-model="model.user.email" class="form-control" id="email" required>
              </div>
              <div class="mb-3">
                <label for="password" class="form-label">Password (Leave blank if not changing)</label>
                <input type="password" v-model="model.user.password" class="form-control" id="password">
              </div>
              <div class="d-flex justify-content-between">
                <button type="submit" class="btn btn-primary">Save</button>
                <RouterLink to="/users" class="btn btn-secondary">Cancel</RouterLink>
              </div>
              <div v-if="errorList.length" class="alert alert-danger mt-3">
                <ul>
                  <li v-for="(error, index) in errorList" :key="index">{{ error }}</li>
                </ul>
              </div>
            </form>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  
  export default {
    name: 'UserEdit',
    data() {
      return {
        model: {
          user: {
            name: '',
            email: '',
            password: ''  // Deja el campo de contraseña vacío
          }
        },
        errorList: [],
        loading: true
      };
    },
    mounted() {
      this.getUserData(this.$route.params.id);
    },
    methods: {
      async getUserData(userId) {
        try {
          const response = await axios.get(`http://127.0.0.1:8000/api/users/${userId}`);
          this.model.user = response.data.User; // Asegúrate de que el campo sea 'user' si tu API responde así
          this.loading = false;
        } catch (error) {
          console.error('An error occurred:', error.message);
          this.loading = false;
        }
      },
      async updateUser() {
        const userId = this.$route.params.id;
        try {
          const response = await axios.put(`http://127.0.0.1:8000/api/users/${userId}`, this.model.user);
          console.log(response.data);
          alert('User updated successfully!');
          this.$router.push('/users');
        } catch (error) {
          if (error.response && error.response.status === 422) {
            this.errorList = error.response.data.errors;
          } else {
            console.error('An error occurred:', error.message);
          }
        }
      }
    }
  };
  </script>
  
  <style scoped>
  .card {
    margin-top: 20px;
  }
  
  .form-label {
    font-weight: 600;
  }
  
  .alert {
    margin-top: 10px;
  }
  </style>
  