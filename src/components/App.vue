<template>
    <div class="terra-collector">
        <NavLine />
        <ContainerFrame>
            <ReactionView :current="current" :get="get" :environment-count="environmentCount" />
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
import ReactionView from './ReactionView.vue';

const current = ref<components["schemas"]["Reaction"]>({
    type: "normal",
    inputs: [{
        count: 1,
        matter: {
            composer: "atom",
            particles: [{
                proton: find("symbol", "Fe").number,
                count: 1
            }]
        }
    }, {
        count: 1,
        matter: {
            composer: "lon",
            particles: [{
                proton: find("symbol", "Cu").number,
                electron: {
                    sign: "+",
                    value: 2
                },
                count: 1,
                valence: {
                    sign: "+",
                    value: 2
                }
            }, {
                proton: find("symbol", "S").number,
                electron: {
                    sign: "+",
                    value: 6
                },
                count: 1
            }, {
                proton: find("symbol", "O").number,
                electron: {
                    sign: "-",
                    value: 2
                },
                count: 4
            }]
        }
    }],
    outputs: [{
        count: 1,
        matter: {
            composer: "lon",
            particles: [{
                proton: find("symbol", "Fe").number,
                electron: {
                    sign: "+",
                    value: 2
                },
                count: 1
            }, {
                proton: find("symbol", "S").number,
                electron: {
                    sign: "+",
                    value: 6
                },
                count: 1
            }, {
                proton: find("symbol", "O").number,
                electron: {
                    sign: "-",
                    value: 2
                },
                count: 4
            }]
        }
    }, {
        count: 1,
        matter: {
            composer: "atom",
            particles: [{
                proton: find("symbol", "Cu").number,
                count: 1
            }]
        }
    }],
    environment: {
        temprature: [{
            min: 0,
            max: 100,
            best: 50
        }, {
            min: 114,
            max: 514,
            best: 200
        }],
        pyro: true,
        cat: [{
            composer: "mole",
            particles: [{
                count: 1,
                proton: find("symbol", "Mn").number
            }, {
                count: 2,
                proton: find("symbol", "O").number
            }]
        }]
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