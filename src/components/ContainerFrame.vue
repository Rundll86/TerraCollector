<template>
    <div class="frame">
        <span class="title">{{ title }}</span>
        <div class="content">
            <slot></slot>
        </div>
    </div>
</template>
<script setup lang="ts">
defineProps({
    title: {
        type: String,
        default: "Container Frame"
    }
});
</script>
<style scoped>
.frame {
    padding: 10px;
    border-radius: 10px;
    border: gray 2px solid;
    text-align: center;
    margin: 10px;
    display: inline-block;
}

.title {
    font-weight: bold;
    font-size: 18px;
    margin-bottom: 10px;
    display: block;
}

.content {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 10px;
}
</style>