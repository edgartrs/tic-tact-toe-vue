<script setup lang="ts">
defineProps({
    value: String,
})
</script>
<template>
    <div
        class="border-2 border-gray-400 w-16 h-16 align-bottom text-center text-4xl/[1.5em]"
    >
        {{ value }}
    </div>
</template>
