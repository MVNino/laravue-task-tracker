<template>
    <div>
        <h4>Expense</h4>
        <p id="money-minus" class="money minus">${{ expense }}</p>
    </div>
</template>

<script setup>
import { defineProps } from 'vue';

const props = defineProps({
    expense: {
        type: Number,
        required: true,
    }
})
</script>