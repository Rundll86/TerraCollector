<template>
    {{ input.count > 1 ? input.count : '' }}
    <ParticleBox v-for="particle in input.matter.particles" :particle="particle" :get="get" />
</template>
<script setup lang="ts">
import { PropType } from "vue";
import { components } from "../structs";
import {elements} from "../elements.json";
import ParticleBox from './ParticleBox.vue';

defineProps({
    input: {
        type: Object as PropType<components["schemas"]["MatterGroup"]>,
        required:true
    },
    get: {
        type: Function as PropType<<K extends keyof typeof elements[number]>(index:number,key:K) => typeof elements[number][K]>,
        required:true
    }
});
</script>