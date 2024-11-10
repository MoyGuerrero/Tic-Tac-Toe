<template>
    <button @click="$emit('click', tipo)" :disabled="isDisabled"
        :class="['text-white dark:bg-blue-400 px-3 py-2 block w-40 text-lg dark:hover:bg-blue-600 cursor-pointer rounded-lg']">
        {{ tipo }}
    </button>
</template>

<script setup lang="ts">
defineProps<{
    tipo: string;
    isDisabled: boolean
}>();

defineEmits(['click']);
</script>