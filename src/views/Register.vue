<script setup>
import { ref } from 'vue';
import { useRouter } from 'vue-router';
import api from '../api'; // Impor instance axios

const name = ref('');
const email = ref('');
const password = ref('');
const confirmPassword = ref('');
const error = ref('');
const success = ref('');
const router = useRouter();

const handleRegister = async () => {
  // Validasi sederhana di sisi klien
  if (!name.value || !email.value || !password.value || !confirmPassword.value) {
    error.value = 'Semua kolom harus diisi!';
    return;
  }
  if (password.value !== confirmPassword.value) {
    error.value = 'Password dan konfirmasi password tidak cocok!';
    return;
  }

  try {
    // Panggilan API ke endpoint register
    const response = await api.post('/register', {
      name: name.value,
      email: email.value,
      password: password.value,
      password_confirmation: confirmPassword.value,
    });

    success.value = 'Pendaftaran berhasil! Mengarahkan ke login...';
    error.value = '';

    // Redirect ke halaman login setelah 2 detik
    setTimeout(() => {
      router.push('/login');
    }, 2000);
  } catch (err) {
    error.value = err.response?.data?.message || 'Pendaftaran gagal! Silakan coba lagi.';
  }
};
</script>

<template>
  <div class="register-container">
    <h2>Register</h2>
    <form @submit.prevent="handleRegister">
      <div>
        <label for="name">Name:</label>
        <input
          type="text"
          id="name"
          v-model="name"
          required
        />
      </div>
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
      <div>
        <label for="confirm-password">Konfirmasi Password:</label>
        <input
          type="password"
          id="confirm-password"
          v-model="confirmPassword"
          required
        />
      </div>
      <button type="submit">Register</button>
      <p v-if="error" class="error">{{ error }}</p>
      <p v-if="success" class="success">{{ success }}</p>
      <p>Sudah punya akun? <router-link to="/login">Login di sini</router-link></p>
    </form>
  </div>
</template>

<style scoped>
.register-container {
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
.success {
  color: green;
  margin-top: 10px;
}
</style>