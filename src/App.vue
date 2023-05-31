<template>
  <h1>Weight Tracker</h1>
  <div class="currentWeight">
    <span>{{ currentWeight.weight }}</span> 
    <p> Current Weight (kg)</p>
  </div>
  <form @submit.prevent="addWeight">
    <input type="number" placeholder="enter your weight..." v-model="weightInput" step="0.1">
    <span>kg</span>
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
            {{ weight.weight }} kg
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

form {
  display: flex;
  border: 1px solid #AAA;
  border-radius: 10px;
  overflow: hidden;
  transition: 200ms linear;
}

form:focus-within,
form:hover {
  border-color:#4f0779;
  border-width: 2px;
}

form input {
  appearance: none;
  outline: none;
  border: none;
  flex: 1 1 0%;
  font-size: 20px;
}

form input[type="number"] {
 background-color: white;
 padding: 20px 25px;
}

form input[type="submit"] {
 cursor: pointer;
 background-color: #4f0779;
 padding: 10px 15px;
 color:white;
 transition: all 200ms linear;
 border-left: 3px solid #4f0779;
}

form input[type="submit"]:hover {
 background-color: #330950;
 color:#e69823;
 border-left: 3px solid #e69823;
}

form span {
  background-color: white;
  padding: 20px 25px;
}

.canvas-box {
  width: 100%;
  max-width: 720px;
  padding: 20px 30px;
  margin: 20px 0;
  background-color: rgb(248, 248, 241);
  border-radius: 10px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
}

.weight-history ul {
  list-style: none;
  padding: 0;
  margin: 0;
}

.weight-history ul li {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px;
  cursor: pointer;
}

.weight-history ul li:nth-child(even) {
  background-color: rgba(255, 105, 180, 0.1);
}

.weight-history ul li:hover {
  background-color: #f8f8f8;
}

.weight-history ul li:last-of-type {
  border-bottom: none;
}

.weight-history ul li span {
  display: block;
  font-size: 15px;
  font-weight: 700;
  margin-left: 20px;
}

.weight-history ul li small {
  color: #888888;
  font-style: italic;
}




</style>
