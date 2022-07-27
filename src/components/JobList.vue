<template>


    <div v-if="selectedCategories.length" class="filter">
        <ul class="categories">
            <li class="category" v-for="category in selectedCategories">
                <span class="category__name">{{ category }}</span>
                <img class="category__remove" @click="removeCategory(category)"
                    src="../../src/assets/images/icon-remove.svg" alt="Remove icon">
            </li>
        </ul>

        <div class="clear">Clear</div>
    </div>

    <div class="result">
        <ul class="jobs">
            <li class="job" v-for="job in filteredJobs" :key="job.id">
                <div class="job__desc">
                    <img :src="job.logo" alt="">
                    {{ job.company }}
                    {{ job.position }}
                    {{ job.postedAt }}
                    {{ job.contract }}
                    {{ job.location }}
                </div>
                <div class="job__categories">
                    <span class="job__category" @click="addCategory(job.role)">{{ job.role }}</span>
                    <span class="job__category" @click="addCategory(job.level)">{{ job.level }}</span>
                    <span class="job__category" @click="addCategory(language)" v-for="language in job.languages">{{
                            language
                    }}</span>
                    <span class="job__category" @click="addCategory(tool)" v-for="tool in job.tools">{{ tool }}</span>
                </div>
            </li>
        </ul>
    </div>
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

.filter {
    height: 72px;
    width: 100%;
    padding: 20px 40px;
    background-color: $color-neutral-White;
    border-radius: 5px;
    position: absolute;
    top: 0;
    left: 0;
    transform: translateY(-50%);
    display: flex;
    align-items: center;
    justify-content: space-between;
}

.categories {
    list-style: none;
    display: flex;
    gap: 10px;
}

.clear {
    cursor: pointer;
    font-weight: 700;
    color: $color-primary-Desaturated-Dark-Cyan;

    &:hover {
        text-decoration: underline;
    }

}

.category {
    display: inline-flex;
    border-radius: 5px;
    overflow: hidden;

    &__name {
        padding: 8px;
        background-color: $color-neutral-Light-Grayish-Cyan-filter;
        color: $color-primary-Desaturated-Dark-Cyan;
    }

    &__remove {
        cursor: pointer;
        padding: 8px;
        background-color: $color-primary-Desaturated-Dark-Cyan;

        &:hover {
            background-color: $color-neutral-Very-Dark-Grayish-Cyan;
        }
    }
}

.result {
    padding-top: 70px;
}

.jobs {
    list-style: none;
    display: flex;
    flex-direction: column;
    gap: 20px;
}

.job {
    width: 100%;
    padding: 20px 40px;
    background-color: $color-neutral-White;
    border-radius: 5px;
    display: flex;
    align-items: center;
    justify-content: space-between;

    &__categories {
        display: flex;
        gap: 5px;
    }

    &__category {
        cursor: pointer;
        padding: 8px;
        border-radius: 5px;
        background-color: $color-neutral-Light-Grayish-Cyan-filter;
        color: $color-primary-Desaturated-Dark-Cyan;

        &:hover {
            background-color: $color-primary-Desaturated-Dark-Cyan;
            color: $color-neutral-White;
        }
    }
}
</style>