<template>
    <div class="terra-collector">
        <NavLine />
        <ContainerFrame title="搜索">
            物品名称：
            <input type="text" v-model="text">
            匹配模式：
            <SelectBar v-model:selected="matchMode" :options="['模糊', '完全']" />
            物品数量：
            <input type="number" v-model="count">
        </ContainerFrame>
        <ContainerFrame title="快捷指令" v-if="selection">
            给其他人：
            /give {{ selection.id }} [玩家名称] {{ count }}
            <br>
            给自己：
            /item {{ selection.id }} {{ count }}
        </ContainerFrame>
        <ContainerFrame title="数据库" class="database">
            <div v-for="item, index in output" @click="selectionId == index ? selectionId = -1 : selectionId = index">
                {{ item.name }}({{ item.identifier }})：{{ item.id }}
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
const count = ref("1");
const selectionId = ref(-1);
const selection = computed<ItemDefinition | null>(() => {
    return output.value[selectionId.value] ?? null;
})

const output = computed(() => {
    return search(text.value, !!matchMode.value);
});
interface ItemDefinition {
    id: string;
    name?: string;
    identifier: string;
}
function search(target: string, exactly: boolean): ItemDefinition[] {
    return database.filter(e => {
        if (exactly) {
            return e.name === target || e.identifier === target || e.id === target;
        } else {
            return e.name?.includes(target) || e.identifier.includes(target);
        }
    });
};
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

.database {
    max-height: 40vh;
    overflow: auto;
}
</style>