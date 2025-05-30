<script setup>
import { onMounted, reactive, ref } from 'vue';
import { RouterLink } from 'vue-router';
import pulseLoader from 'vue-spinner/src/PulseLoader.vue';
import Single_job from './single_job.vue';
import axios from 'axios';

defineProps({
    limit: Number,
    showButton: {
        type: Boolean,
        default: false
    }
});

const state = reactive({
    jobs: [],
    isLoading: true
});

onMounted(async () => {
    const url = import.meta.env.VITE_API_URL;
    try {
        const res = await axios.get(url + 'jobs');
        state.jobs = res.data;
        // console.log(res.data);
    } catch (error) {
        console.log("Error fetching");
    } finally {
        state.isLoading = false;
    }
});
</script>

<template>
    <section class="bg-blue-50 px-4 py-10">
        <div class="container-xl lg:container m-auto">
            <h2 class="text-3xl font-bold text-green-500 mb-6 text-center">Browse Jobs</h2>
            <div v-if="state.isLoading" class="text-center text-gray-500 py-6">
                <pulseLoader />
            </div>
            <div v-else class="grid grid-cols-1 md:grid-cols-3 gap-6">
                <Single_job v-for="job in state.jobs.slice(0, limit || state.jobs.length)" :key="job.id" :job="job"/>
            </div>
        </div>
        <section v-if="showButton" class="m-auto max-w-lg my-10 px-6">
            <RouterLink
            to="/jobs"
            class="block bg-black text-white text-center py-4 px-6 rounded-xl hover:bg-gray-700"
            >View All Jobs</RouterLink>
        </section>
    </section>
</template>
