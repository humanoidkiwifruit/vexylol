<template>
    <a :href="`/flags/${flag.slug}`" class="result-item-link">
        <div class="result-item">
            <img
                :src="`/flag-svgs/${flag.isoAlpha2Name.toLowerCase()}.svg`"
                :alt="`${flag.name} flag`"
                class="flag-image"
            />
            <span class="flag-name">{{ flag.name }}</span>
        </div>
    </a>
</template>

<script setup lang="ts">
import type { Flag } from '../types';
const props = defineProps<{
    flag: Flag
}>();
</script>

<style scoped>

.result-item {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 4px;
    padding: 8px;
    border: 1px solid;
    border-radius: 4px;
}

.flag-image {
    height: 24px;
    width: auto;
    object-fit: contain;
}
.flag-name {
    font-size: 14px;
}
</style>
