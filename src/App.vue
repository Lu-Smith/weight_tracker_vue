<template>
<h1>Weight Tracker</h1>
<form @submit.prevent="addWeight">
<input type="number" placeholder="enter your weight..." v-model="currentWeight" step="0.1"><span>kg</span>
<input type="submit" value="Add weight">
</form>
<p>{{ currentWeight }} kg</p>
</template>

<script setup >
  import { ref} from 'vue'

  const weights = ref([])
  const currentWeight = ref(null)

  const addWeight = () => {
    weights.value.push({
      weight: currentWeight.value,
      date: new Date().getTime()
    })
  }
  
</script>

<style>
input {
 padding: 10px 20px;
}

</style>
