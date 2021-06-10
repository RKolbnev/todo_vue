<template>
  <div class="form__wrap">
    <div class="form">
      <div class="form__header">
        <div>
          <img src="../assets/todo-logo.png" alt="">
        </div>
        <p>Добро пожаловать!</p>
        <span>Что бы продолжить введите свой логин</span>
      </div>
      <div class="form__content">
        <input type="text" placeholder="Логин" v-model.trim="login">
        <button>Продолжить</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      login: ''
    }
  }
}
</script>

<style>

</style>
