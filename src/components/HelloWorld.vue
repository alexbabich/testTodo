<template>
  <b-container class="hello mt-4 mb-4">
    <h1>{{ msg }}</h1>
  </b-container>
</template>

<script>
export default {
  name: 'HelloWorld',
  data () {
    return {
      msg: 'todo list'
    }
  }
}
</script>

<style scoped>
  * {
    font-size: 14px;
  }
</style>
