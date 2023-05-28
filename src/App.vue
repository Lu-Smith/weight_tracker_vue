<template>
<h1>hello world</h1>
<input type="number" placeholder="enter your weight in kg..." v-model="weight">
<p>{{ weight }} kg</p>
</template>

<script setup >
  import { ref} from 'vue'

  // const weights = ref([])
  const weight = ref(null)
  
</script>

<styles>

</styles>
