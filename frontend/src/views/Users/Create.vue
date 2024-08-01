<template>
    <div class="container mt-5">
      <div class="card">
        <div class="card-header">
          <h4>Add New User</h4>
        </div>
        <div class="card-body">
          <form @submit.prevent="saveUser">
            <div class="mb-3">
              <label for="name" class="form-label">Name</label>
              <input type="text" v-model="model.user.name" class="form-control" id="name" required>
            </div>
            <div class="mb-3">
              <label for="email" class="form-label">Email</label>
              <input type="email" v-model="model.user.email" class="form-control" id="email" required>
            </div>
            <div class="mb-3">
              <label for="password" class="form-label">Password</label>
              <input type="password" v-model="model.user.password" class="form-control" id="password" required>
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
  </template>
  
  <script>
  import axios from 'axios';
  
  export default {
    name: 'UserCreate',
    data() {
      return {
        model: {
          user: {
            name: '',
            email: '',
            password: ''
          }
        },
        errorList: []
      };
    },
    methods: {
      async saveUser() {
        try {
          const response = await axios.post('http://127.0.0.1:8000/api/users', this.model.user);
          console.log(response.data);
  
          // Muestra un mensaje de confirmación
          alert('User added successfully!');
  
          // Resetea los campos del formulario
          this.model.user = {
            name: '',
            email: '',
            password: '' // Asegúrate de mantener el campo de contraseña vacío después del envío
          };
  
          // Limpia la lista de errores
          this.errorList = [];
  
          // Redirige a la vista de usuarios
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
  