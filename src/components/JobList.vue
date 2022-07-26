<template>
    <ul>
        <li class="job" v-for="job in jobs" :key="job.id">
            <img :src="job.logo" alt="">
            {{ job.company }}
            {{ job.position }}
            {{ job.postedAt }}
            {{ job.contract }}
            {{ job.location }}
        </li>
    </ul>
</template>

<script setup>
import { ref } from 'vue'
import json from '../assets/data.json'

const jobs = ref(json)

</script>

<style lang="scss" scoped>
.job {
    background: $color-neutral-White;
}
</style>