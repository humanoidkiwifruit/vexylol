<template>
    <h1>Search</h1>

    <input
        v-model="searchTerm"
        type="text"
        placeholder="Search..."
        class="main-search-input"
    />

    <div class="checkbox-group">
        <div
            v-for="colour in availableColours"
            :key="colour"
            class="checkbox-item"
        >
            <input
                type="checkbox"
                :id="`${colour}_checkbox`"
                :value="colour"
                v-model="activeColours"
            />
            <label :for="`${colour}_checkbox`">{{ colourLookups[colour][1] }}</label>
        </div>
    </div>

    <div class="results-grid">
        <FlagCard v-for="flag in filteredFlags" :key="flag.slug" :flag="flag" />
    </div>
</template>

<script setup lang="ts">

import { ref, computed, onMounted } from "vue";
import FlagCard from "./FlagCard.vue";
import { colourLookups } from "../colours";
import type { Flag } from "../types";


const flagsJson = ref<Flag[]>([]); //holds flags.json
const searchTerm = ref("");
const activeColours = ref([]); //holds the list of selected colour filters; ["red", "black"]

onMounted(async () => {
    const response = await fetch("/flags.json");
    flagsJson.value = await response.json();
});

const availableColours = Object.keys(colourLookups); //List of possible colours that the user might want to select

const filteredFlags = computed(() => { //Whittle down flags.json into only what the user wants to see
    const query = searchTerm.value.toLowerCase();
    return flagsJson.value.filter((flag) => {
        const matchesSearch = flag.slug.toLowerCase().includes(query);
      if (activeColours.value.length === 0) return matchesSearch;
        const matchesColours = activeColours.value.every((colour) =>
            flag.mainColours?.includes(colour),
        );
        return matchesSearch && matchesColours;
    });
});

</script>

<style scoped>
.checkbox-group {
    display: flex;
    gap: 12px;
    margin: 12px 0;
}
.checkbox-item {
    display: flex;
    align-items: center;
    gap: 4px;
}
.results-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(120px, 1fr));
    gap: 16px;
    margin-top: 16px;
}
</style>
