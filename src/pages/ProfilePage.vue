<template>
  <div class="page">
    <h1>Профиль пользователя</h1>
    <p>Твоя роль: <strong>{{ authStore.role }}</strong></p>
    
    <button v-if="authStore.isAdmin" @click="$router.push('/admin')" class="btn-admin">
      Перейти в панель администратора
    </button>
    <button @click="logout" class="btn-danger">Выйти</button>

    <!--Вариант 16: Кнопка запроса данных -->
    <hr style="margin: 2rem 0;">
    <h3>Запрос данных для пользователя</h3>
    <button @click="fetchUserData" :disabled="loading">
      {{ loading ? 'Загрузка...' : 'Запросить данные' }}
    </button>
    <div v-if="data">
      <pre>{{ data }}</pre>
    </div>
    <p v-if="error" class="error">{{ error }}</p>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import { useAuthStore } from '../stores/auth'
import { useRouter } from 'vue-router'

const authStore = useAuthStore()
const router = useRouter()
const loading = ref(false)
const data = ref(null)
const error = ref(null)

const logout = () => {
  authStore.logout()
  router.push('/login')
}

//вариант 16
const fetchUserData = async () => {
  loading.value = true
  error.value = null
  data.value = null

  if (!authStore.validateToken()) {
    error.value = 'Сессия истекла. Войдите снова.'
    loading.value = false
    return
  }

  try {
    const response = await fetch('https://jsonplaceholder.typicode.com/users/1', {
      headers: {
        'Authorization': `Bearer ${authStore.token}`,
        'Content-Type': 'application/json'
      }
    })
    if (!response.ok) throw new Error(`Ошибка: ${response.status}`)
    data.value = await response.json()
  } catch (err) {
    error.value = err.message
  } finally {
    loading.value = false
  }
}
</script>