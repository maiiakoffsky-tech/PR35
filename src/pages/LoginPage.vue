<template>
  <div class="page">
    <h2>Вход</h2>
    <form @submit.prevent="handleLogin">
      <input v-model="username" placeholder="Логин" />
      <input v-model="password" type="password" placeholder="Пароль" />
      <button type="submit">Войти</button>
      <p v-if="error" class="error">{{ error }}</p>
    </form>
    <p>Тестовые данные: admin/admin123, user/user123</p>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import { useAuthStore } from '../stores/auth'
import { useRouter } from 'vue-router'

const username = ref('')
const password = ref('')
const error = ref('')
const authStore = useAuthStore()
const router = useRouter()

const handleLogin = () => {
  try {
    authStore.login({ username: username.value, password: password.value })
    router.push(authStore.isAdmin ? '/admin' : '/profile')
  } catch (e) {
    error.value = e.message
  }
}
</script>