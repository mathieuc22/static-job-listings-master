<template>
    <div v-if="selectedCategories.length" class="categories">
        <span @click="removeCategory(category)" v-for="category in selectedCategories">{{ category }}</span>
    </div>
    <ul>
        <li class="job" v-for="job in jobs" :key="job.id">
            <img :src="job.logo" alt="">
            {{ job.company }}
            {{ job.position }}
            {{ job.postedAt }}
            {{ job.contract }}
            {{ job.location }}
            <div class="categories">
                <span @click="addCategory(job.role)">{{ job.role }}</span>
                <span @click="addCategory(job.level)">{{ job.level }}</span>
                <span @click="addCategory(language)" v-for="language in job.languages">{{ language }}</span>
                <span @click="addCategory(tool)" v-for="tool in job.tools">{{ tool }}</span>
            </div>
        </li>
    </ul>
</template>

<script setup>
import { ref } from 'vue'
import json from '../assets/data.json'

const jobs = ref(json)
const selectedCategories = ref([])

function addCategory(category) {
    // prevent duplicates
    if (!selectedCategories.value.includes(category)) {
        selectedCategories.value.push(category)
    }
}

function removeCategory(category) {
    const index = selectedCategories.value.indexOf(category);
    // only splice array when item is found
    if (index > -1) {
        selectedCategories.value.splice(index, 1);
    }
}

</script>

<style lang="scss" scoped>
.job {
    background: $color-neutral-White;
}

.categories {
    display: flex;
    gap: 5px;
}
</style>