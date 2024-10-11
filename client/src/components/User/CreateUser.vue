<template>
  <div class="create-user-container">
    <h1 class="centered-title">Create User</h1>
    <form v-on:submit.prevent="createUser" class="create-user-form">
      <div class="form-group">
        <label>Name:</label>
        <input type="text" v-model="user.name" required />
      </div>
      <div class="form-group">
        <label>Lastname:</label>
        <input type="text" v-model="user.lastname" required />
      </div>
      <div class="form-group">
        <label>Email:</label>
        <input type="email" v-model="user.email" required />
      </div>
      <div class="form-group">
        <label>Password:</label>
        <input type="password" v-model="user.password" required />
      </div>
      <div>
        <button type="submit" class="submit-button">Create User</button>
      </div>
      <div v-if="errorMessage" class="error">{{ errorMessage }}</div>
    </form>
  </div>
</template>

<script>
import UsersService from '../../services/UsersService';

export default {
  data() {
    return {
      user: {
        name: '',
        lastname: '',
        email: '',
        password: '',
        status: 'active'
      },
      errorMessage: '' // กำหนด errorMessage ใน data
    }
  },
  
  methods: {
    async createUser() {
      this.errorMessage = ''; // Reset error message

      // Log user data for debugging
      console.log("Creating user with data:", this.user);

      try {
        const response = await UsersService.post(this.user);
        console.log("User created successfully:", response); // Log response
        this.$router.push('/users');
      } catch (err) {
        // Handle error and set the error message
        this.errorMessage = 'เกิดข้อผิดพลาดในการสร้างผู้ใช้ กรุณาลองใหม่';
        console.error("Error creating user:", err);
      }
    }
  }
};
</script>

<style scoped>
html, body {
  height: 100%;
  margin: 0;
}

.create-user-container {
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

.create-user-form {
  background-color: rgb(255, 255, 255); /* พื้นหลังของฟอร์ม */
  padding: 20px;
  border-radius: 10px; /* มุมมน */
  box-shadow: 0 2px 10px rgb(0, 0, 0); /* เงา */
  width: 300px; /* กำหนดความกว้าง */
}

.form-group {
  margin-bottom: 15px; /* ระยะห่างระหว่างฟิลด์ */
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
