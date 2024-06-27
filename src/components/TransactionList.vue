<template>
    <h3>History</h3>
     <ul id="list" class="list">
        <li v-for="transaction in transactions" :key="transaction.id" :class="transaction.amount<0 ? 'minus':'plus'">
            {{ transaction.text }} <span>Rs {{ transaction.amount }}</span>
            <button @click ="deleteTransaction(transaction.id)" class="delete-btn">X</button>
        </li>
     </ul>
</template>



<script setup>
const emit = defineEmits(['deletedTransaction'])

import {defineProps} from 'vue'
const props = defineProps({transactions:{
    type: Array,
    required: true
}})
const deleteTransaction = (id)=>
{
    emit('deleteTransaction',id)

}


</script>
