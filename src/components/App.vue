<template>
    <div class="terra-collector">
        <NavLine />
        <ContainerFrame title="搜索">
            物品名称：
            <textarea type="text" v-model="text"></textarea>
            匹配模式：
            <SelectBar v-model:selected="matchMode" :options="['模糊', '完全']" />
        </ContainerFrame>
        <div>
            <ContainerFrame title="快捷指令" class="protect-height">
                把这个物品给谁？
                <SelectBar :options="['自己', '其他人']" v-model:selected="commandMode" />
                <div class="list">
                    <div v-for="target in output" class="output">
                        <OutLink v-for="item in target.items" class="item" @click="copy(generateCommand(target, item))">
                            <template #prompt>
                                {{ item.name }} × {{ target.count }}
                            </template>
                            {{ generateCommand(target, item) }}
                        </OutLink>
                        <span v-if="!target.items.length">
                            <span class="bold">
                                {{ target.name }}
                            </span>
                            无物品源，无法生成快捷指令
                        </span>
                    </div>
                </div>
            </ContainerFrame>
            <ContainerFrame title="数据库" class="protect-height">
                <span v-if="!output.length">
                    尚未记录任何条目
                </span>
                <div class="list">
                    <div v-for="target in output" class="output">
                        <span class="bold">搜索结果：{{ target.name }} × {{ target.count }}</span>
                        <span v-for="item in target.items" class="item">
                            <OutLink :href="`https://terraria.wiki.gg/zh/${item.name}`">
                                <template #prompt>
                                    点击跳转Wiki页面：{{ item.name }}({{ item.identifier }})
                                </template>
                                {{ item.name }}：{{ item.id }}
                            </OutLink>
                            <br>
                        </span>
                        <span v-if="!target.items.length">
                            没有搜索到相关物品
                        </span>
                    </div>
                </div>
            </ContainerFrame>
        </div>
        <FootBar />
    </div>
</template>
<script setup lang="ts">
import { computed, ref } from "vue";
import ContainerFrame from "./ContainerFrame.vue";
import NavLine from "./NavLine.vue";
import SelectBar from "./SelectBar.vue";
import FootBar from "./FootBar.vue";
import database from "../items.json";
import OutLink from "./OutLink.vue";
const text = ref("");
const matchMode = ref(0);
const commandMode = ref(0);

const output = computed(() => searchGroup(text.value, !!matchMode.value));

interface ItemDefinition {
    id: string;
    name?: string;
    identifier: string;
}
interface SearchTarget {
    items: ItemDefinition[];
    count: number;
    name: string;
}
function search(target: string, exactly: boolean): ItemDefinition[] {
    if (!target?.trim()) return [];
    return database.filter(e => {
        if (exactly) {
            return e.name?.toLowerCase() === target.toLowerCase() || e.identifier.toLowerCase() === target.toLowerCase() || Number(e.id) === Number(target);
        } else {
            return e.name?.toLowerCase().includes(target.toLowerCase()) || e.identifier.toLowerCase().includes(target.toLowerCase());
        }
    });
}
function searchGroup(targets: string, exactly: boolean): SearchTarget[] {
    return targets.split(/[\n,;]/gi).filter(Boolean).map(target => {
        const targetName = target.split("*")[0].trim();
        const count = Number(target.split("*")[1] ?? 1);
        return {
            items: search(targetName, exactly),
            count: Number.isNaN(count) || count <= 0 ? 1 : count,
            name: targetName
        };
    });
}
function generateCommand(target: SearchTarget, item: ItemDefinition) {
    if (commandMode.value === 0) {
        return `/item ${item.id} ${target.count}`;
    } else {
        return `/give ${item.id} [玩家名称] ${target.count}`;
    }
}
async function copy(data: string) {
    try {
        await navigator.clipboard.writeText(data);
        alert("复制成功");
    } catch {
        alert("复制失败");
    }
}
</script>
<style scoped>
.terra-collector {
    padding: 80px 0;
    display: flex;
    flex-direction: column;
    align-items: center;
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