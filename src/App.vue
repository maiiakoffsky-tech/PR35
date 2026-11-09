<template>
  <header>
    <div>
      <strong>PR35 Auth App</strong>
    </div>
    <div>
      <span v-if="authStore.isAuthenticated">
        Авторизован: <strong>{{ authStore.role }}</strong>
        <RouterLink to="/profile">Профиль</RouterLink>
        <RouterLink v-if="authStore.isAdmin" to="/admin">Админка</RouterLink>
      </span>
      <span v-else>
        <RouterLink to="/login">Войти</RouterLink>
      </span>
    </div>
  </header>
  <RouterView />
</template>

<script setup>
import { useAuthStore } from './stores/auth'
const authStore = useAuthStore()
</script>