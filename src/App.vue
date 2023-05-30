<template>
<h1>Weight Tracker</h1>
<div>
  <p>{{ currentWeight }}</p> 
  <p> Current Weight (kg)</p>
</div>
<form @submit.prevent="addWeight">
<input type="number" placeholder="enter your weight..." v-model="weightInput" step="0.1"><span>kg</span>
<input type="submit" value="Add weight">
</form>
<div v-if="weights && weights.length > 0">
  <h2>Last 7 days</h2>
  <div class="canvas-box">
    <canvas ref="weightChartEl"></canvas>
  </div>
  <div class="weight-history">
    <h2>Weight History</h2>
    <ul>
      <li v-for="weight in weights" :key="weight.date">
        <span>
          {{ weight.weight }}kg
        </span>
        <small>{{ new Date(weight.date).toLocaleDateString() }}</small>
      </li>
    </ul>
  </div>
</div>
</template>

<script setup >
  import { computed, ref} from 'vue'

  const weights = ref([])
  const currentWeight = computed(() => {
    return weights.value.sort((a, b) => b.date - a.date)[0] || 0
  })
  const weightInput = ref(60.0)
  const weightChartEl = ref(null)
  
  const addWeight = () => {
    weights.value.push({
      weight: weightInput.value,
      date: new Date().getTime()
    })
  }
  
</script>

<style>
input {
 padding: 10px 20px;
}

.canvas-box {
  background-color: red ;
}

</style>
