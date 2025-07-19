<template>
    <div class="footbar">
        <OutLink href="https://github.com/Rundll86/TerraCollector">TerraCollector</OutLink>
        v{{ packager.version }}
        <OutLink href="https://rundll86.github.io">@F-Shrimp</OutLink>
    </div>
</template>
<script setup lang="ts">
import packager from "../../package.json";
import OutLink from "./OutLink.vue";
</script>
<style scoped>
.footbar {
    position: fixed;
    left: 0;
    bottom: 0;
    width: 100vw;
    padding: 10px;
    background-color: rgb(230, 230, 230);
    text-align: center;
}
</style>