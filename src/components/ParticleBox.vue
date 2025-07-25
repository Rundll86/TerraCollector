<template>
    <span>
        {{ get(particle.proton ?? 1, "symbol") }}
    </span>
    <span class="index" v-if="particle.count > 1">
        {{ particle.count }}
    </span>
</template>
<script setup lang="ts">
import { PropType } from "vue";
import { components } from "../structs";
import {elements} from "../elements.json";
defineProps({
    particle: {
        type: Object as PropType<components["schemas"]["Particle"]>,
        required:true
    },
    get: {
        type: Function as PropType<<K extends keyof typeof elements[number]>(index:number,key:K) => typeof elements[number][K]>,
        required:true
    }
});
</script>
<style scoped>
span {
    display: inline-block;
}

.index {
    font-size: 10px;
}
</style>