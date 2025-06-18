<script setup>
import { ref } from 'vue';
import { useRouter } from 'vue-router';
import api from '../api'; // Impor instance axios

const email = ref('');
const password = ref('');
const error = ref('');
const router = useRouter();

const handleLogin = async () => {
  try {
    const response = await api.post('/login', {
      email: email.value,
      password: password.value,
    });

    // Simpan token atau status autentikasi
    localStorage.setItem('isAuthenticated', 'true');
    if (response.data.token) {
      localStorage.setItem('token', response.data.token);
    }

    router.push('/dashboard');
  } catch (err) {
    error.value = err.response?.data?.message || 'Email atau password salah!';
  }
};
</script>

<template>
  <div class="login-container">
    <h2>Login</h2>
    <form @submit.prevent="handleLogin">
      <div>
        <label for="email">Email:</label>
        <input
          type="email"
          id="email"
          v-model="email"
          required
        />
      </div>
      <div>
        <label for="password">Password:</label>
        <input
          type="password"
          id="password"
          v-model="password"
          required
        />
      </div>
      <button type="submit">Login</button>
      <p v-if="error" class="error">{{ error }}</p>
      <p>Belum punya akun? <router-link to="/register">Register di sini</router-link></p>
    </form>
  </div>
</template>

<style scoped>
.login-container {
  max-width: 400px;
  margin: 50px auto;
  padding: 20px;
  border: 1px solid #ccc;
  border-radius: 5px;
}
form div {
  margin-bottom: 15px;
}
label {
  display: block;
  margin-bottom: 5px;
}
input {
  width: 100%;
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
}
button {
  width: 100%;
  padding: 10px;
  background-color: #42b983;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}
button:hover {
  background-color: #3aa876;
}
.error {
  color: red;
  margin-top: 10px;
}
</style>