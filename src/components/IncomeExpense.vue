<template>
  <div class="inc-exp-container">
    <div>
        <h4>Income</h4>
        <p id="money-plus" class="money plus">Rs {{ income }}</p>
    </div>
    <div>
        <h4>Expense</h4>
        <p id="money-minus" class="money minus">Rs {{ expense }}</p>
    </div>
  </div>
</template>


<script setup>
import {defineProps} from 'vue'
const props = defineProps({
  income:{
    type: [Number,String],
    required:true
  },
  expense:{
    type: [Number,String],
    required: true
  }
})
</script>