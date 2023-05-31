<template>
<h1>Weight Tracker</h1>
<div class="currentWeight">
  <span>{{ currentWeight.weight }}</span> 
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
  import { computed, nextTick, ref, watch, shallowRef} from 'vue'
  import Chart from 'chart.js/auto'

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

  const weightChart = shallowRef(null)

  watch(weights, newWeights => {
    const ws = [...newWeights]

    if (weightChart.value) {
      weightChart.value.data.labels = ws
            .sort((a,b) => a.date - b.date)
            .map(w => new Date(w.date).toLocaleDateString())
            .slice(-7)

      weightChart.value.data.datasets[0].data = ws
            .sort((a,b) => a.date - b.date)
            .map(w => w.weight)
            .slice(-7)

      weightChart.value.update()

      return
    }
    
    nextTick(() => {
      weightChart.value = new Chart(weightChartEl.value.getContext('2d'), {
        type: 'line',
        data: {
          labels: ws
            .sort((a,b) => a.date - b.date)
            .map(w => new Date(w.date).toLocaleDateString()),
          datasets: [
            {
              label: 'Weights',
              data: ws
              .sort((a,b) => a.date - b.date)
              .map(w => w.weight),
              backgroundColor: 'rgba(255, 105, 180, 0.2)',
              borderColor: 'rgba(255, 105, 180, 0.9)',
              borderWidth: 1,
              fill: true
            }
          ]
        },
        options: {
          responsive: true,
          maintainAspectRatio: false
        }
      })
    })
    console.log(ws)
  }, {deep: true})
  
</script>

<style>
input {
 padding: 10px 20px;
}

.currentWeight {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  width: 200px;
  height: 200px;
  text-align: center;
  background-color: white;
  border-radius: 999px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  border: 5px solid #e69823;
  margin: 30px auto;
  color: #4f0779;
  

}

.currentWeight span {
  display: block;
  font-size: 36px;
  font-weight: bold;
  margin-bottom: 10px;
}

.canvas-box {
  margin: 20px 0;
  background-color: rgb(248, 248, 241) ;
}

</style>
