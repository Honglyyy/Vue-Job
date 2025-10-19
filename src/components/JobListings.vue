<script setup>
import { ref ,defineProps, onMounted, reactive} from 'vue';
import JobListing from './JobListing.vue';
import { RouterLink } from 'vue-router';
import axios from 'axios';
import MoonLoader from 'vue-spinner/src/MoonLoader.vue'


const state = reactive({
    jobs: [],
    isLoading: true
});
onMounted(async () => {
    try {
        const response = await axios.get('/api/jobs');
        state.jobs = response.data;
    } catch (error) {
        console.log(error)
    }
    finally{
        state.isLoading = false;
    }
})
defineProps({
    limit: Number,
    showBtn: {
        type: Boolean,
        default: false
    }

})


// console.log(jobs.value)
</script>
<template>
    
    <section class="bg-blue-50 px-4 py-10">
        <div class="container-xl lg:container m-auto">
            <h2 class="text-3xl font-bold text-green-500 mb-6 text-center">
                Browse Jobs
            </h2>

            <div v-if="state.isLoading" class="absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2">

                <MoonLoader />

            </div>

            <div class="grid grid-cols-1 gap-6 md:grid-cols-3">
                <JobListing v-for="job in state.jobs.slice(0, limit || state.jobs.length)" :key="job.id" :job="job"/>
            </div>
        </div>
    </section>
     <section class="m-auto max-w-lg my-10 px-6" v-if="showBtn">
      <RouterLink
        to="/jobs"
        class="block bg-black text-white text-center py-4 px-6 rounded-xl hover:bg-gray-700"
        >View All Jobs</RouterLink
      >
    </section>
</template>