<template>
    <div class="terra-collector">
        <NavLine />
        <ContainerFrame title="搜索">
            物品名称：
            <textarea type="text" v-model="text"></textarea>
            匹配模式：
            <SelectBar v-model:selected="matchMode" :options="['模糊', '完全']" />
        </ContainerFrame>
        <ContainerFrame title="快捷指令" class="protect-height">
            把这个物品给谁？
            <SelectBar :options="['自己', '其他人']" v-model:selected="commandMode" />
            <div v-for="target in output">
                <div v-for="item in target.items">
                    <span v-if="commandMode === 1">
                        /give {{ item.id }} [玩家名称] {{ target.count }}
                    </span>
                    <span v-else>
                        /item {{ item.id }} {{ target.count }}
                    </span>
                </div>
            </div>
        </ContainerFrame>
        <ContainerFrame title="数据库" class="protect-height">
            <div v-for="target in output">
                <div v-for="item in target.items">
                    {{ item.name }}({{ item.identifier }})：{{ item.id }}
                </div>
            </div>
        </ContainerFrame>
        <FootBar />
    </div>
</template>
<script setup lang="ts">
import { computed, onMounted, ref } from "vue";
import ContainerFrame from "./ContainerFrame.vue";
import NavLine from "./NavLine.vue";
import SelectBar from "./SelectBar.vue";
import FootBar from "./FootBar.vue";
import database from "../items.json";
const text = ref("");
const matchMode = ref(0);
const commandMode = ref(0);

const output = computed(() => searchGroup(text.value, !!matchMode.value));
interface ItemDefinition {
    id: string;
    name?: string;
    identifier: string;
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
function searchGroup(targets: string, exactly: boolean) {
    return targets.split(/[\n,;]/gi).map(target => ({
        items: search(target.split("*")[0], exactly),
        count: Number(target.split("*")[1] ?? 1)
    }));
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

.output {
    max-width: 50%;
}

.protect-height {
    max-height: 40vh;
    overflow: auto;
}
</style>