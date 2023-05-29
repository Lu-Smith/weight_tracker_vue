<template>
<h1>hello world</h1>
<input type="number" placeholder="enter your weight..." v-model="currentWeight">
<span>kg</span>
<p>{{ currentWeight }} kg</p>
</template>

<script setup >
  import { ref} from 'vue'

  // const weights = ref([])
  const currentWeight = ref(null)
  
</script>

<style>
input {
 padding: 10px 20px;
}

</style>
