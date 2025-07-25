<template>
    <div class="cont">
        <MatterView :view="current.inputs" :get="get" />
        <span class="env" :style="{ '--h': `${environmentCount}rem` }">
            <span v-if="current.environment.pyro">△</span><br>
            <span v-for="temp in [...current.environment.temprature ?? []]">
                {{ temp.min }}
                ~
                {{ temp.max }}°C
                <br>
            </span>
            <span v-for="temp in [...current.environment.cat ?? []]">
                <MatterGroup :input="{ matter: temp, count: 1 }" :get="get" />
                <br>
            </span>
        </span>
        <MatterView :view="current.outputs" :get="get" />
    </div>
</template>
<script setup lang="ts">
import { PropType } from 'vue';
import MatterView from './MatterView.vue';
import { components } from 'src/structs';
import {elements} from "../elements.json";
import MatterGroup from './MatterGroup.vue';
defineProps({
    current: {
        type: Object as PropType<components["schemas"]["Reaction"]>,
        required: true
    },
    environmentCount: {
        type: Number,
        required: true
    },
    get:{
        type:Function as PropType<<K extends keyof typeof elements[number]>(index:number,key:K) => typeof elements[number][K]>,
        required:true
    },
});
</script>
<style scoped>
.env {
    --h: 0rem;
    position: relative;
    display: inline-block;
    transform: translateY(-75%);
    min-width: 16px;
    margin: 0 5px;
    font-size: 14px;
    margin-top: var(--h);
}

.env::before,
.env::after {
    content: "";
    position: absolute;
    left: 0;
    width: 100%;
    height: 1px;
    background-color: black;
}

.env::before {
    top: calc(100%);
}

.env::after {
    top: calc(100% + 0.3rem);
}

.cont {
    display: flex;
    align-items: baseline;
}
</style>