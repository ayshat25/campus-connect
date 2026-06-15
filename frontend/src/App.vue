<script setup lang="ts">
import { ref } from 'vue'

const email = ref('')
const password = ref('')
const message = ref('')

const register = async () => {
  message.value = ''

  try {
    const res = await fetch('http://localhost:3000/auth/register', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({
        email: email.value,
        password: password.value,
      }),
    })

    const data = await res.json()

    if (!res.ok) {
      message.value = data.message || 'Error'
      return
    }

    message.value = `User created: ${data.email}`
  } catch (err) {
    message.value = 'Server error'
  }
}
</script>

<template>
  <div style="max-width: 400px; margin: 100px auto; font-family: sans-serif;">
    <h2>Campus Connect</h2>

    <input v-model="email" placeholder="Email" style="display:block; width:100%; margin-bottom:10px; padding:8px;" />
    
    <input v-model="password" type="password" placeholder="Password" style="display:block; width:100%; margin-bottom:10px; padding:8px;" />

    <button @click="register" style="padding:10px; width:100%;">
      Register
    </button>

    <p style="margin-top:10px;">{{ message }}</p>
  </div>
</template>