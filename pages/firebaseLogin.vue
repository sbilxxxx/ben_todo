<template>
  <div class="container">
      <button class="button is-primary is-rounded" @click="login">
        ログイン
      </button>
  </div>
</template>

<script>
export default {
  methods: {
    login() {
      console.log('login')
      // this.$store.dispatch('login')
    }
  }
}
</script>

<style>
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}
</style>
