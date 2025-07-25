<template>
    <div v-for="input, index in view">
        <MatterGroup :input="input" :get="get" />
        <StateLabel :state="index < view.length - 1" truly="+" falsey="" />
    </div>
</template>
<script setup lang="ts">
import { PropType, ref } from 'vue';
import { components } from "../structs";
import MatterGroup from './MatterGroup.vue';
import StateLabel from './StateLabel.vue';
import {elements} from "../elements.json";
const props = defineProps({
    view:{
        type: Array as PropType<components["schemas"]["MatterGroup"][]>,
        required:true
    },
    get: {
        type:Function as PropType<<K extends keyof typeof elements[number]>(index:number,key:K) => typeof elements[number][K]>,
        required:true
    },
});
</script>