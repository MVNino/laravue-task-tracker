<template>
    <div>
        <h4>Income</h4>
        <p id="money-plus" class="money plus">${{ income }}</p>
    </div>
</template>

<script setup>
import { defineProps } from "vue";

const props = defineProps({
    income: {
        type: Number,
        required: true
    }
})
</script>