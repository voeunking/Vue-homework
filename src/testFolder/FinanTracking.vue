<template>
  <div class="min-h-screen bg-slate-100 p-6">
    <div class="max-w-7xl mx-auto">

      <!-- Header -->
      <h1 class="text-4xl font-bold text-slate-800 mb-8">
         Personal Finance Tracker
      </h1>

      <!-- Add Transaction Form -->
      <div class="bg-white rounded-2xl shadow-lg p-6 mb-8">
        <div class="grid md:grid-cols-5 gap-4">

          <input
            v-model="form.desc"
            type="text"
            placeholder="Description"
            class="w-full px-4 py-3 border rounded-xl focus:ring-2 focus:ring-blue-500 outline-none"
          />
          
          <input
            v-model.number="form.amount"
            type="number"
            placeholder="Amount"
            class="w-full px-4 py-3 border rounded-xl focus:ring-2 focus:ring-blue-500 outline-none"
          />

          <select
            v-model="form.type"
            class="w-full px-4 py-3 border rounded-xl focus:ring-2 focus:ring-blue-500 outline-none"
          >
            <option value="income">Income</option>
            <option value="expense">Expense</option>
          </select>

          <input
            v-model="form.date"
            type="date"
            class="w-full px-4 py-3 border rounded-xl focus:ring-2 focus:ring-blue-500 outline-none"
          />

          <button
            @click="addTransaction"
            class="bg-blue-600 hover:bg-blue-700 text-white font-semibold rounded-xl transition"
          >
            Add
          </button>

        </div>
      </div>

      <!-- Summary Cards -->
      <div class="grid md:grid-cols-3 gap-6 mb-8">

        <div class="bg-white rounded-2xl shadow-lg p-6 border-l-4 border-green-500">
          <p class="text-gray-500">Total Income</p>
          <h2 class="text-3xl font-bold text-green-600">
            ${{ totalIncome }}
          </h2>
        </div>

        <div class="bg-white rounded-2xl shadow-lg p-6 border-l-4 border-red-500">
          <p class="text-gray-500">Total Expenses</p>
          <h2 class="text-3xl font-bold text-red-600">
            ${{ totalExpenses }}
          </h2>
        </div>

        <div class="bg-white rounded-2xl shadow-lg p-6 border-l-4 border-blue-500">
          <p class="text-gray-500">Balance</p>
          <h2
            class="text-3xl font-bold"
            :class="balance >= 0 ? 'text-blue-600' : 'text-red-600'"
          >
            ${{ balance }}
          </h2>
        </div>

      </div>

      <!-- Budget Section -->
      <div class="bg-white rounded-2xl shadow-lg p-6 mb-8">

        <div class="flex justify-between items-center mb-3">
          <h3 class="font-semibold text-lg">Budget Progress</h3>
          <span
            class="font-semibold"
            :class="isOverBudget ? 'text-red-500' : 'text-green-500'"
          >
            {{ budgetStatus }}
          </span>
        </div>

        <div class="w-full bg-gray-200 rounded-full h-6 overflow-hidden">
          <div
            class="bg-green-500 h-6 text-xs text-white flex items-center justify-center transition-all duration-500"
            :style="{ width: expensePercentage + '%' }"
          >
            {{ expensePercentage.toFixed(0) }}%
          </div>
        </div>

      </div>

      <!-- Filter -->
      <div class="flex justify-between items-center mb-4">

        <select
          v-model="filterType"
          class="px-4 py-2 bg-white border rounded-xl shadow-sm"
        >
          <option value="all">All Transactions</option>
          <option value="income">Income</option>
          <option value="expense">Expense</option>
        </select>

        <button
          @click="clearAll"
          class="bg-yellow-500 hover:bg-yellow-600 text-white px-5 py-2 rounded-xl font-medium"
        >
          Clear All
        </button>

      </div>

      <!-- Transactions Table -->
      <div class="bg-white rounded-2xl shadow-lg overflow-hidden mb-8">

        <table class="w-full">

          <thead class="bg-slate-800 text-white">
            <tr>
              <th class="p-4 text-left">Description</th>
              <th class="p-4 text-left">Amount</th>
              <th class="p-4 text-left">Type</th>
              <th class="p-4 text-left">Date</th>
              <th class="p-4 text-left">Action</th>
            </tr>
          </thead>

          <tbody>

            <tr
              v-for="item in filteredTransactions"
              :key="item.id"
              class="border-b hover:bg-slate-50"
            >
              <td class="p-4">{{ item.desc }}</td>

              <td class="p-4 font-semibold">
                ${{ item.amount }}
              </td>

              <td class="p-4">
                <span
                  class="px-3 py-1 rounded-full text-sm font-medium"
                  :class="
                    item.type === 'income'
                      ? 'bg-green-100 text-green-700'
                      : 'bg-red-100 text-red-700'
                  "
                >
                  {{ item.type }}
                </span>
              </td>

              <td class="p-4">
                {{ item.date }}
              </td>

              <td class="p-4">
                <button
                  @click="deleteTransaction(item.id)"
                  class="bg-red-500 hover:bg-red-600 text-white px-3 py-1 rounded-lg"
                >
                  Delete
                </button>
              </td>
            </tr>

          </tbody>

        </table>

      </div>

      <!-- Bottom Section -->
      <div class="grid md:grid-cols-2 gap-6">

        <!-- Category Summary -->
        <div class="bg-white rounded-2xl shadow-lg p-6">
          <h3 class="text-xl font-bold mb-4">
             Category Summary
          </h3>

          <div
            v-for="(value, key) in categorySummary"
            :key="key"
            class="flex justify-between bg-slate-50 p-3 rounded-lg mb-2"
          >
            <span>{{ key }}</span>
            <span class="font-semibold text-red-500">
              ${{ value }}
            </span>
          </div>
        </div>

        <!-- Notifications -->
        <div class="bg-white rounded-2xl shadow-lg p-6">
          <h3 class="text-xl font-bold mb-4">
            Notifications
          </h3>

          <div
            v-for="(log, index) in notificationLog"
            :key="index"
            class="bg-orange-50 border-l-4 border-orange-500 p-3 rounded-lg mb-2"
          >
            {{ log }}
          </div>

          <p
            v-if="notificationLog.length === 0"
            class="text-gray-400"
          >
            No notifications yet.
          </p>
        </div>

      </div>

    </div>
  </div>
</template>
<script setup>
import { ref, computed, watch, onMounted } from "vue";

const transactions = ref([]);

const form = ref({
  desc: "",
  amount: 0,
  type: "income",
  date: "",
});

const filterType = ref("all");
const budgetLimit = ref(1000);
const notificationLog = ref([]);

onMounted(() => {
  const saved = localStorage.getItem("transactions");

  if (saved) {
    transactions.value = JSON.parse(saved);
  }
});

const filteredTransactions = computed(() => {
  if (filterType.value === "all") {
    return transactions.value;
  }

  return transactions.value.filter(
    (item) => item.type === filterType.value
  );
});

const totalIncome = computed(() => {
  return transactions.value
    .filter((item) => item.type === "income")
    .reduce((sum, item) => sum + item.amount, 0);
});

const totalExpenses = computed(() => {
  return transactions.value
    .filter((item) => item.type === "expense")
    .reduce((sum, item) => sum + item.amount, 0);
});

const balance = computed(() => {
  return totalIncome.value - totalExpenses.value;
});

const isOverBudget = computed(() => {
  return totalExpenses.value > budgetLimit.value;
});

const expensePercentage = computed(() => {
  return Math.min(
    (totalExpenses.value / budgetLimit.value) * 100,
    100
  );
});

const categorySummary = computed(() => {
  const summary = {};

  transactions.value.forEach((item) => {
    if (item.type === "expense") {
      summary[item.desc] =
        (summary[item.desc] || 0) + item.amount;
    }
  });

  return summary;
});

const budgetStatus = computed(() => {
  return isOverBudget.value
    ? "Over Budget"
    : "Within Budget";
});

const addTransaction = () => {
  if (
    !form.value.desc ||
    !form.value.amount ||
    !form.value.date
  ) {
    alert("Please fill all fields");
    return;
  }

  transactions.value.push({
    id: Date.now(),
    ...form.value,
  });

  form.value = {
    desc: "",
    amount: 0,
    type: "income",
    date: "",
  };
};

const deleteTransaction = (id) => {
  transactions.value = transactions.value.filter(
    (item) => item.id !== id
  );
};

const clearAll = () => {
  transactions.value = [];
};

watch(
  transactions,
  (value) => {
    localStorage.setItem(
      "transactions",
      JSON.stringify(value)
    );
  },
  { deep: true }
);

watch(balance, (newBalance) => {
  if (newBalance < 0) {
    alert("Balance is negative!");
  }
});

watch(isOverBudget, (value) => {
  if (value) {
    notificationLog.value.push(
      `Budget exceeded at ${new Date().toLocaleString()}`
    );
  }
});
</script>