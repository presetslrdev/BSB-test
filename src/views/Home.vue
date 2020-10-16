<template>
  <div class="home">
    <h3>Главная</h3>
    <router-link :to="{name: 'Register'}">Зарегистрировать пользователя</router-link>
  </div>
</template>

<script>

export default {
  name: 'Home',
  components: {
  }
}
</script>
