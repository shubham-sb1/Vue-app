<script setup>
import {ref, defineProps, onMounted, reactive } from 'vue'
import JobListing from './JobListing.vue';
import { RouterLink } from 'vue-router';
import axios from 'axios';

defineProps({
    limit: Number,
    showButton: {
        type: Boolean,
        default: false,
    },

});

const state = reactive({
    jobs: [],

});


onMounted(async () => {
try{
    const response = await axios.get('/api/jobs');
    state.jobs= response.data;
}catch(error){
    console.error("Error fetching jobs", error);
}
});
</script>

<template>
    <section class="bg-blue-50 px-4 py-10">
        <div class = "container-xl lg:container m-auto ">
            <h1 class = 'text-xl font-bold text-green-500 mb-6 text-center'>
                Browse Jobs 
            </h1>
            <div class = 'grid grid-cols-1 md:grid-cols-3 gap 6'>
                <JobListing  v-for="job in state.jobs.slice(0,limit || state.jobs.length)" :key="job.id" :job="job"/>
            </div>

        </div>

    </section>
    <section v-if="showButton" class="m-auto max-w-lg my-10 px-6">
      <RouterLink
        to="/jobs"
        class="block bg-black text-white text-center py-4 px-6 rounded-xl hover:bg-gray-700"
        >View All Jobs</RouterLink
      >
    </section>

</template>