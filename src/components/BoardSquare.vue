<script setup lang="ts">
import { ref } from "vue"
defineProps({
    value: String,
    highlight: Boolean,
})
const isClicked = ref(false)
</script>
<template>
    <div
        class="border-2 border-gray-400 w-16 h-16 align-bottom text-center text-4xl/[1.5em] hover:cursor-pointer"
        :class="{
            'bg-green-200': highlight,
            'bg-blue-200': isClicked,
        }"
        @mousedown="isClicked = true"
        @mouseup="isClicked = false"
        @mouseleave="isClicked = false"
    >
        {{ value }}
    </div>
</template>
