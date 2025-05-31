<script setup>
import { ref, onMounted } from 'vue'

const name = ref('')
const cost = ref('')
const goals = ref([])

onMounted(() => {
  getGoals()
})

const getGoals = async () => {
  try {
    const response = await fetch(`${import.meta.env.VITE_API_BASE_URL}/testgoals/get-goals`, {
      method: 'GET',
      headers: {
        'Content-Type': 'application/json',
      },
    })

    if (!response.ok) throw new Error('Failed to create goal')

    const data = await response.json()
    goals.value = data
    console.log(data)
  } catch (error) {
    console.error('Retrieval error:', error)
  }
}

const submitGoal = async () => {
  try {
    const response = await fetch(`${import.meta.env.VITE_API_BASE_URL}/testgoals/create-goal`, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({
        name: name.value,
        cost: parseFloat(cost.value),
      }),
    })

    if (!response.ok) throw new Error('Failed to create goal')

    const data = await response.json()
    console.log('Goal created:', data)
  } catch (error) {
    console.error('Submission error:', error)
  }
}
</script>

<template>
  <div class="test-api">
    <h1>Test the API Below</h1>
    <form @submit.prevent="submitGoal" action="">
      <input v-model="name" type="text" placeholder="Goal name" />
      <input v-model="cost" type="number" placeholder="Goal cost" />
      <button type="submit">Submit</button>
    </form>
    <div>
      <p v-if="!goals.length">No goals found</p>
      <ul v-else>
        <li v-for="goal in goals" :key="goal.id">{{ goal.name }} - {{ goal.cost }}</li>
      </ul>
    </div>
  </div>
</template>

<style>
@media (min-width: 1024px) {
  .about {
    min-height: 100vh;
    display: flex;
    align-items: center;
  }

  form {
    display: flex;
    flex-direction: column;
  }
}
</style>
