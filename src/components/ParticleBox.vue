<template>
    <span class="particle" :class="{ right: particle.electron }">
        {{ get(particle.proton ?? 1, "symbol") }}
        <span class="valence" v-if="particle.valence">
            {{ particle.valence.sign + Math.abs(particle.valence.value) }}
        </span>
        <span class="electron" v-if="particle.electron">
            {{ Math.abs(particle.electron.value) + particle.electron.sign }}
        </span>
        <span class="index" v-if="particle.count > 1">
            {{ particle.count }}
        </span>
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
.particle {
    display: inline-block;
    position: relative;
}

.particle.right {
    margin-right: 15px;
}

.valence,
.electron,
.index {
    position: absolute;
}

.valence {
    left: 50%;
    top: -100%;
    font-size: 14px;
    transform: translateX(-50%);
}

.electron {
    left: 100%;
    top: -2px;
    font-size: 10px;
}

.index {
    left: 100%;
    bottom: -2px;
    font-size: 10px;
}
</style>