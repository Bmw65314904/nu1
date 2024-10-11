<template>
  <div class="login-container">
    <h1 class="centered-title">User Login</h1>
    <form v-on:submit.prevent="onLogin" class="login-form">
      <div class="form-group">
        <label for="email">Email</label>
        <input type="email" id="email" name="email" v-model="email" required />
      </div>
      <div class="form-group">
        <label for="password">Password</label>
        <div class="password-container">
          <input
            :type="passwordVisible ? 'text' : 'password'"
            id="password"
            name="password"
            v-model="password"
            required
          />
          <button type="button" @click="togglePasswordVisibility">
            {{ passwordVisible ? 'Hide' : 'Show' }}
          </button>
        </div>
      </div>
      <button type="submit" class="submit-button">Login</button>
      <div class="error" v-if="error">{{ error }}</div>
    </form>
  </div>
</template>

<script>
import AuthenService from "../services/AuthenService";

export default {
  data() {
    return {
      email: "",
      password: "",
      error: null,
      passwordVisible: false,
    };
  },
  methods: {
    async onLogin() {
      try {
        const response = await AuthenService.login({
          email: this.email,
          password: this.password,
        });

        this.$store.dispatch("setToken", response.data.token);
        this.$store.dispatch("setUser", response.data.user);

        this.$router.push({ name: "users" });
      } catch (error) {
        console.log(error);
        this.error = error.response.data.error;
        this.email = "";
        this.password = "";
      }
    },
    togglePasswordVisibility() {
      this.passwordVisible = !this.passwordVisible;
    },
  },
};
</script>

<style scoped>
html, body {
  height: 100%;
  margin: 0;
}

.login-container {
  height: 100vh; /* ใช้ความสูงเต็มหน้าจอ */
  display: flex;
  flex-direction: column;
  justify-content: center; /* จัดกลางในแนวตั้ง */
  align-items: center; /* จัดกลางในแนวนอน */
  background-color: #76ffc8; /* พื้นหลังอ่อน */
  font-family: Arial, sans-serif; /* ฟอนต์ */
}

.centered-title {
  margin-bottom: 20px; /* ระยะห่างด้านล่าง */
  font-size: 2em; /* ขนาดฟอนต์ */
  color: #000000; /* สีข้อความ */
}

.login-form {
  background-color: white; /* พื้นหลังของฟอร์ม */
  padding: 20px;
  border-radius: 10px; /* มุมมน */
  box-shadow: 0 2px 10px rgb(0, 0, 0); /* เงา */
  width: 300px; /* กำหนดความกว้าง */
}

.form-group {
  margin-bottom: 15px; /* ระยะห่างระหว่างฟิลด์ */
}

.password-container {
  display: flex; /* จัดให้ปุ่มแสดง/ซ่อนอยู่ข้าง input */
  align-items: center;
}

.password-container button {
  margin-left: 10px; /* เพิ่มระยะห่างระหว่าง input กับปุ่ม */
}

.submit-button {
  background-color: #007bff; /* สีพื้นหลังปุ่ม */
  color: white; /* สีข้อความในปุ่ม */
  border: none; /* ไม่มีขอบ */
  padding: 10px;
  border-radius: 5px; /* มุมมน */
  cursor: pointer; /* เปลี่ยนเคอร์เซอร์ */
  width: 100%; /* กว้างเต็มฟอร์ม */
}

.submit-button:hover {
  background-color: #0056b3; /* สีเมื่อเอาเมาส์ไปเหนือ */
}

.error {
  color: red;
  margin-top: 10px; /* เพิ่มระยะห่างระหว่าง error กับปุ่ม */
}
</style>
