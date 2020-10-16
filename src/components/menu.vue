<template>
  <nav class="uk-navbar-container" uk-navbar>
    <div class="uk-navbar-left">
      <ul class="uk-navbar-nav">
        <router-link :to="{name: 'Register'}" tag="li"><a href="#">Регистрация</a></router-link>
<!--        <router-link :to="{name: 'Edit'}" tag="li"><a href="#">Редактирование</a></router-link>-->
        <router-link :to="{name: 'UserList'}" tag="li"><a href="#">Список пользователей</a></router-link>
      </ul>
    </div>
  </nav>
</template>

<script>
    export default {
        name: "menu"
    }
</script>

<style scoped>

</style>