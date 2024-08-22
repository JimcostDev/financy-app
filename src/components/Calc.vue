<template>
    <section class="flex flex-col md:flex-row justify-center items-start min-h-screen bg-gray-100 p-8">
      <!-- Formulario de Transacciones -->
      <div class="bg-white p-8 rounded-lg shadow-lg w-full max-w-md mb-8 md:mb-0 md:mr-8">
        <h1 class="text-3xl font-bold mb-8 text-center text-indigo-600 uppercase tracking-wide">Financy APP</h1>
        <form @submit.prevent="addTransaction">
          <div class="mb-6">
            <label class="block text-gray-800 text-sm font-semibold mb-2" for="amount">
              Valor
            </label>
            <input v-model="amount" type="number" id="amount" class="border-2 border-gray-300 rounded-lg w-full py-2 px-4 focus:ring-2 focus:ring-indigo-500 focus:border-indigo-500" required>
          </div>
          <div class="mb-6">
            <label class="block text-gray-800 text-sm font-semibold mb-2" for="description">
              Descripción
            </label>
            <input v-model="description" type="text" id="description" class="border-2 border-gray-300 rounded-lg w-full py-2 px-4 focus:ring-2 focus:ring-indigo-500 focus:border-indigo-500" required>
          </div>
          <div class="mb-6">
            <label class="block text-gray-800 text-sm font-semibold mb-2">
              Tipo de Transacción
            </label>
            <div class="flex items-center">
              <label class="inline-flex items-center mr-6">
                <input v-model="type" type="radio" value="income" class="form-radio text-indigo-600" required>
                <span class="ml-2 text-gray-800">Ingreso</span>
              </label>
              <label class="inline-flex items-center">
                <input v-model="type" type="radio" value="expense" class="form-radio text-indigo-600">
                <span class="ml-2 text-gray-800">Gasto</span>
              </label>
            </div>
          </div>
          <div class="flex justify-center">
            <button type="submit" class="bg-indigo-600 hover:bg-indigo-800 text-white font-semibold py-2 px-6 rounded-lg focus:outline-none focus:ring-2 focus:ring-indigo-500">
              Agregar Transacción
            </button>
          </div>
        </form>
        <div class="mt-8">
          <h2 class="text-xl font-semibold mb-4 text-indigo-600">Historial</h2>
          <ul class="space-y-4">
            <li v-for="(transaction, index) in transactions" :key="index" class="flex justify-between items-center p-4 bg-gray-50 rounded-lg shadow-sm">
              <span :class="transaction.type === 'income' ? 'text-green-600' : 'text-red-600'">
                {{ transaction.amount }} - {{ transaction.description }}
              </span>
              <span :class="transaction.type === 'income' ? 'text-green-600' : 'text-red-600'">
                {{ transaction.type === 'income' ? 'Ingreso' : 'Gasto' }}
              </span>
            </li>
          </ul>
        </div>
      </div>
  
      <!-- Resumen -->
      <article class="bg-white rounded-lg shadow-lg w-full max-w-md p-8">
        <h2 class="text-2xl font-bold mb-8 text-center text-indigo-600 uppercase tracking-wide">Resumen</h2>
        <div class="text-lg space-y-4">
          <p>Total de Ingresos: <span class="text-green-600 font-semibold">{{ totalIncome }}</span></p>
          <p>Total de Gastos: <span class="text-red-600 font-semibold">{{ totalExpenses }}</span></p>
          <p class="font-semibold py-4 px-4 bg-gray-50 rounded-lg shadow-sm">Total General: <span :class="totalBalance >= 0 ? 'text-green-600' : 'text-red-600'">{{ totalBalance }}</span></p>
        </div>
      </article>
    </section>
  </template>
  
  <script>
  export default {
    data() {
      return {
        amount: '',
        description: '',
        type: '',
        transactions: []
      };
    },
    computed: {
      totalIncome() {
        return this.transactions
          .filter(transaction => transaction.type === 'income')
          .reduce((sum, transaction) => sum + parseFloat(transaction.amount), 0);
      },
      totalExpenses() {
        return this.transactions
          .filter(transaction => transaction.type === 'expense')
          .reduce((sum, transaction) => sum + parseFloat(transaction.amount), 0);
      },
      totalBalance() {
        return this.totalIncome - this.totalExpenses;
      }
    },
    methods: {
      addTransaction() {
        if (this.amount && this.description && this.type) {
          this.transactions.push({
            amount: parseFloat(this.amount),
            description: this.description,
            type: this.type,
            date: new Date()
          });
          this.amount = '';
          this.description = '';
          this.type = '';
        }
      }
    }
  };
  </script>
  
  <style>
  body {
    margin: 0;
    font-family: 'Nunito', sans-serif;
  }
  </style>
  