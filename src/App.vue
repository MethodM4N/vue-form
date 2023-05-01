<template>
  <Form />
</template>

<script>
import Form from '@/components/Form.vue';

export default {
  name: 'App',
  components: { Form },
};
</script>

<style>
#app {
  width: 100%;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  display: flex;
  justify-content: center;
}

body {
  margin: 0 auto;
  background-color: rgb(250, 244, 237);
}
</style>
