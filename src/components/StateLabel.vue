<template>
    <span v-if="state">{{ truly }}</span>
    <span v-else>{{ falsey }}</span>
</template>
<script setup lang="ts">
defineProps({
    state: Boolean,
    truly: String,
    falsey: String,
});
</script>