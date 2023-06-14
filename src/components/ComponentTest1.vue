<script setup lang="ts">
const test: string = 'TEST'

const props = defineProps({
  helloMsg: String
})

const emit = defineEmits(['notification'])

</script>

<template>
  <div class="hello">
    <p>{{ test }}</p>
    <p>{{ props['helloMsg'] }}</p>
    <button @click="emit('notification')">通知</button>
    <hr>
  </div>
</template>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  div p {
    text-align: center;
  }

  button {
    display: block;
    margin: 10px auto;
  }
</style>
