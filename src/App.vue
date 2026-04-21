<script setup>
import { ref, computed } from 'vue'
const categories = ['Food', 'Transport', 'Entertainment', 'Bills', 'Other']

const expenses = ref([])
const description = ref('')
const amount = ref('')
const category = ref(categories[0])

const addExpense = () => {
  if (description.value.trim() === '' || amount.value <= 0) return
  expenses.value.push({
    id: Date.now(),
    description: description.value,
    amount: amount.value,
    category: category.value,
  })
  description.value = ''
  amount.value = ''
  category.value = categories[0]
}

const removeExpense = (id) => {
  expenses.value = expenses.value.filter((expense) => expense.id !== id)
}

const totalAmount = computed(() => {
  return expenses.value.reduce((sum, expense) => sum + expense.amount, 0)
})
</script>

<template>
  <h1>Expense Tracker</h1>
  <h2>Total amount: {{ totalAmount }} CZK</h2>
  <h3 v-if="expenses.length > 0">
    You logged {{ expenses.length }} {{ expenses.length === 1 ? 'expense' : 'expenses' }}
  </h3>
  <input type="text" v-model="description" />
  <input type="text" v-model.number="amount" />
  <select name="category" id="category" v-model="category">
    <option v-for="cat in categories" :value="cat" :key="cat">{{ cat }}</option>
  </select>
  <button @click="addExpense">Add</button>
  <ul>
    <li v-for="expense in expenses" :key="expense.id">
      <span>{{ expense.description }}: {{ expense.amount }} CZK</span><br />
      <span>{{ expense.category }}</span
      ><br />
      <button @click="removeExpense(expense.id)">Remove</button>
    </li>
  </ul>
</template>

<style scoped></style>
