<template>
<h1>Weight Tracker</h1>
<input type="number" placeholder="enter your weight..." v-model="currentWeight" step="0.1">
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
