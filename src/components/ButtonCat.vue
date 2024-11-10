<template>
    <button @click="$emit('click', position)" :disabled="isDisabled" class="bg-neutral-100 h-full w-full hover:bg-slate-300 rounded-lg">{{
        value
        }}</button>
</template>

<script setup lang="ts">
defineProps<{
    position: number;
    value: string;
    isDisabled:boolean;
}>();

defineEmits(['click'])
</script>

<style scoped></style>