<template>
    <div v-if="selectedCategories.length" class="categories">
        <span @click="removeCategory(category)" v-for="category in selectedCategories">{{ category }}</span>
    </div>
    <ul>
        <li class="job" v-for="job in filteredJobs" :key="job.id">
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
import { ref, onMounted } from 'vue'
import json from '../assets/data.json'

const selectedCategories = ref([])
const filteredJobs = ref([])

onMounted(() => {
    // init list of jobs
    filteredJobs.value = json
})

// recursive filter function based on selected categories array
function filter() {
    // init filtered jobs with all jobs
    filteredJobs.value = json
    // filter based on selected categories
    if (selectedCategories.value.length) {
        selectedCategories.value.forEach(category => {
            filteredJobs.value = filteredJobs.value.filter(job =>
                job.languages.includes(category) ||
                job.tools.includes(category) ||
                job.role === category ||
                job.level === category
            );
        });
    }
}

function addCategory(category) {
    // prevent duplicates
    if (!selectedCategories.value.includes(category)) {
        selectedCategories.value.push(category)
    }
    // refresh list of jobs
    filter()
}

function removeCategory(category) {
    const index = selectedCategories.value.indexOf(category);
    // only splice array when item is found
    if (index > -1) {
        selectedCategories.value.splice(index, 1);
    }
    // refresh list of jobs
    filter()
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