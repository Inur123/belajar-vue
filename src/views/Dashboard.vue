<template>
  <div class="dashboard">
    <h2>Selamat Datang di Dashboard!</h2>
    <button @click="handleLogout">Logout</button>
  </div>
</template>

<script>
export default {
  methods: {
    handleLogout() {
      // Hapus status autentikasi
      localStorage.removeItem('isAuthenticated');
      // Redirect ke login
      this.$router.push('/login');
    }
  }
};
</script>

<style scoped>
.dashboard {
  text-align: center;
  margin-top: 50px;
}
button {
  padding: 10px 20px;
  background-color: #ff4444;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}
button:hover {
  background-color: #cc0000;
}
</style>