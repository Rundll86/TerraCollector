<template>
    <div class="terra-collector">
        <NavLine />
        <ContainerFrame>
            <div class="cont">
                <MatterView :view="current.inputs" :get="get" />
                <span class="env" :style="{ '--h': `${environmentCount}rem` }">
                    <span v-if="current.environment.pyro">▲</span><br>
                    <span v-for="temp in [...current.environment.temprature ?? []]">
                        {{ temp.min }}
                        ~
                        {{ temp.max }}°C
                    </span>
                </span>
                <MatterView :view="current.outputs" :get="get" />
            </div>
        </ContainerFrame>
        <FootBar />
    </div>
</template>
<script setup lang="ts">
import { computed, ref } from 'vue';
import { components } from "../structs";
import info from "../elements.json";
import ContainerFrame from './ContainerFrame.vue';
import NavLine from './NavLine.vue';
import FootBar from './FootBar.vue';
import MatterView from './MatterView.vue';

const current = ref<components["schemas"]["Reaction"]>({
    type: "normal",
    inputs: [{
        count: 1,
        matter: {
            composer: "atom",
            particles: [{
                proton: find("symbol", "Fe")?.number,
                count: 1
            }]
        }
    }, {
        count: 1,
        matter: {
            composer: "lon",
            particles: [{
                proton: find("symbol", "Cu")?.number,
                electron: -2,
                count: 1
            }, {
                proton: find("symbol", "S")?.number,
                electron: +6,
                count: 1
            }, {
                proton: find("symbol", "O")?.number,
                electron: -2,
                count: 4
            }]
        }
    }],
    outputs: [{
        count: 1,
        matter: {
            composer: "lon",
            particles: [{
                proton: find("symbol", "Fe")?.number,
                electron: +2,
                count: 1
            }, {
                proton: find("symbol", "S")?.number,
                electron: -6,
                count: 1
            }, {
                proton: find("symbol", "O")?.number,
                electron: +2,
                count: 4
            }]
        }
    }, {
        count: 1,
        matter: {
            composer: "atom",
            particles: [{
                proton: find("symbol", "Cu")?.number,
                count: 1
            }]
        }
    }],
    environment: {
        temprature: [{
            min: 0,
            max: 100,
            best: 50
        }],
        pyro: true,
        cat: []
    }
});
const environmentCount = computed(() => {
    return (current.value.environment.temprature?.length ?? 0) + Number(current.value.environment.pyro) + (current.value.environment.cat?.length ?? 0);
});
function get<K extends keyof typeof info.elements[number]>(index: number, key: K): typeof info.elements[number][K] {
    return info.elements[index][key];
}
function find<K extends keyof typeof info.elements[number]>(key: K, value: typeof info.elements[number][K]) {
    return info.elements[info.elements.findIndex((item) => item[key] === value) - 1];
}
</script>
<style scoped>
.terra-collector {
    padding: 80px 0;
    display: flex;
    align-items: center;
    justify-content: center;
}

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
}

.preview {
    width: 300px;
    border: 2px solid gray;
}

button {
    border: 2px solid gray;
    padding: 5px 10px;
    border-radius: 5px;
}

button:hover {
    border-color: rgb(68, 68, 68);
    cursor: pointer;
}

input,
textarea {
    border: gray 2px solid;
    transition: none;
    padding: 3px 5px;
    border-radius: 5px;
}

.protect-height {
    max-height: 40vh;
    overflow: scroll;
}

.protect-height::-webkit-scrollbar {
    display: none;
}

.list {
    display: flex;
    flex-direction: column;
    gap: 5px;
}

.output {
    border: 1px solid gray;
    padding: 10px;
    display: flex;
    flex-direction: column;
}

.item {
    text-wrap: nowrap;
}

.item:hover {
    background-color: rgba(0, 0, 0, 0.1);
}

.item:active {
    background-color: rgba(0, 0, 0, 0.2);
}

.bold {
    font-weight: bold;
    margin-bottom: 5px;
}
</style>