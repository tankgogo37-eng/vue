
<script setup>
import { ref } from 'vue'

const email = ref('')
const password = ref('')
const rememberMe = ref(false)
const showPassword = ref(false)

function login() {
  if (!email.value || !password.value) {
    alert('Please enter your email and password!')
    return
  }

  // Demo login only - connect to backend for real authentication
  alert('Login form submitted successfully!')

  console.log({
    email: email.value,
    password: password.value,
    rememberMe: rememberMe.value
  })
}
</script>

<template>
  <div class="login-page">

    <div class="login-card">

      <!-- Logo -->
      <div class="logo">
        <div class="logo-icon">V</div>
        <h2>Vue<span>System</span></h2>
      </div>

      <!-- Heading -->
      <div class="login-heading">
        <h1>Welcome Back!</h1>
        <p>Please login to your account</p>
      </div>

      <!-- Login Form -->
      <form @submit.prevent="login">

        <!-- Email -->
        <div class="form-group">
          <label>Email Address</label>

          <input
            v-model="email"
            type="email"
            placeholder="Enter your email"
            required
          />
        </div>

        <!-- Password -->
        <div class="form-group">
          <label>Password</label>

          <div class="password-box">
            <input
              v-model="password"
              :type="showPassword ? 'text' : 'password'"
              placeholder="Enter your password"
              required
            />

            <button
              type="button"
              class="show-password"
              @click="showPassword = !showPassword"
            >
              {{ showPassword ? 'Hide' : 'Show' }}
            </button>
          </div>
        </div>

        <!-- Remember and Forgot -->
        <div class="form-options">

          <label class="remember">
            <input
              v-model="rememberMe"
              type="checkbox"
            />
            Remember me
          </label>

          <a href="#" @click.prevent="alert('Please contact administrator')">
            Forgot Password?
          </a>

        </div>

        <!-- Login Button -->
        <button type="submit" class="login-btn">
          Login
        </button>

      </form>

      <!-- Register -->
      <div class="register">
        Don't have an account?
        <a href="#">Create Account</a>
      </div>

      <!-- Footer -->
      <div class="footer">
        © 2026 VueSystem. All rights reserved.
      </div>

    </div>

  </div>
</template>

<style scoped>
* {
  box-sizing: border-box;
}

.login-page {
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 20px;
  background: linear-gradient(135deg, #dbeafe, #eff6ff, #e0e7ff);
  font-family: Arial, sans-serif;
}

.login-card {
  width: 100%;
  max-width: 430px;
  background: white;
  padding: 40px;
  border-radius: 18px;
  box-shadow: 0 15px 45px rgba(0, 0, 0, 0.1);
}

/* Logo */
.logo {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 10px;
  margin-bottom: 30px;
}

.logo-icon {
  width: 40px;
  height: 40px;
  background: #2563eb;
  color: white;
  border-radius: 10px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 23px;
  font-weight: bold;
}

.logo h2 {
  font-size: 23px;
  color: #1e293b;
}

.logo h2 span {
  color: #2563eb;
}

/* Heading */
.login-heading {
  text-align: center;
  margin-bottom: 30px;
}

.login-heading h1 {
  font-size: 28px;
  color: #1e293b;
  margin-bottom: 10px;
}

.login-heading p {
  color: #64748b;
  font-size: 14px;
}

/* Form */
.form-group {
  margin-bottom: 20px;
}

.form-group label {
  display: block;
  margin-bottom: 8px;
  color: #334155;
  font-weight: 600;
  font-size: 14px;
}

.form-group input {
  width: 100%;
  padding: 13px;
  border: 1px solid #cbd5e1;
  border-radius: 8px;
  outline: none;
  font-size: 14px;
}

.form-group input:focus {
  border-color: #2563eb;
  box-shadow: 0 0 0 3px #2563eb20;
}

/* Password */
.password-box {
  position: relative;
}

.password-box input {
  padding-right: 65px;
}

.show-password {
  position: absolute;
  right: 10px;
  top: 50%;
  transform: translateY(-50%);
  border: none;
  background: none;
  color: #2563eb;
  cursor: pointer;
  font-weight: 600;
}

/* Options */
.form-options {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 8px;
  margin-bottom: 25px;
  font-size: 12px;
}

.remember {
  display: flex;
  align-items: center;
  gap: 5px;
  color: #475569;
}

.form-options a,
.register a {
  color: #2563eb;
  text-decoration: none;
  font-weight: 600;
}

/* Button */
.login-btn {
  width: 100%;
  padding: 14px;
  border: none;
  border-radius: 8px;
  background: #2563eb;
  color: white;
  font-size: 16px;
  font-weight: bold;
  cursor: pointer;
  transition: 0.3s;
}

.login-btn:hover {
  background: #1d4ed8;
  transform: translateY(-2px);
}

/* Register */
.register {
  text-align: center;
  margin-top: 25px;
  font-size: 13px;
  color: #64748b;
}

/* Footer */
.footer {
  border-top: 1px solid #e2e8f0;
  margin-top: 30px;
  padding-top: 20px;
  text-align: center;
  font-size: 11px;
  color: #94a3b8;
}

/* Responsive */
@media (max-width: 480px) {
  .login-card {
    padding: 25px 20px;
  }

  .login-heading h1 {
    font-size: 24px;
  }
}
</style>
