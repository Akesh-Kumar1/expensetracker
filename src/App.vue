<template>
  <Header />
  <div class="container">
    <Balance :total="+total"></Balance>
    <Expenditure :income="+income" :expense="+expense"></Expenditure>
    <History :transactions="transactions" @deleteTransaction="handledeleteTransaction"></History>
    <Transaction @transactionSubmitted="handleTransactionSubmitted"></Transaction>
  </div>
</template>

<script setup>
import Header from './components/Header.vue'
import Balance from './components/Balance.vue'
import Expenditure from './components/IncomeExpense.vue'
import History from './components/TransactionList.vue'
import Transaction from './components/AddTransaction.vue'
import {ref,computed, onMounted} from 'vue'
import {useToast} from 'vue-toastification'

const toast = useToast()

const transactions = ref( [

      ]);

onMounted(()=>{
  const savedTransactions = JSON.parse(localStorage.getItem('transactions'))
   if(savedTransactions)
   {
    transactions.value = savedTransactions;

   }
})
//get total
const total = computed(()=>{
  return transactions.value.reduce((acc,transaction)=>{
         return acc+transaction.amount
  },0);
})
//get income
const income = computed(()=>{
  return transactions.value
  .filter((transaction)=>transaction.amount>0)
  .reduce((acc,transaction)=>{
         return acc+transaction.amount
  },0).toFixed(2);
})

//get expense
const expense = computed(()=>{
  return transactions.value
  .filter((transaction)=>transaction.amount<0)
  .reduce((acc,transaction)=>{
         return acc+transaction.amount
  },0).toFixed(2);
})
//add transaction
const handleTransactionSubmitted = (transactionData)=>{
    transactions.value.push({id:generateUniqueId(),
      text: transactionData.text,
      amount : transactionData.amount
    })
    savedTransactionsToLocalStorage();
    toast.success('Transaction Added')
}
//generate unique id
const generateUniqueId = ()=>{
  return Math.floor(Math.random()*1000000)
}
//delete transaction
const handledeleteTransaction =(id)=>{
  transactions.value = transactions.value.filter((transaction)=>transaction.id!==id)
  savedTransactionsToLocalStorage();
  toast.success('Transaction Deleted')
}
//save to localstorage
const savedTransactionsToLocalStorage = ()=>{
  localStorage.setItem('transactions',JSON.stringify(transactions.value))
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
