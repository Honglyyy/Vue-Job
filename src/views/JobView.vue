<script setup>
import axios from 'axios'
import { reactive, onMounted } from 'vue'
import MoonLoader from 'vue-spinner/src/MoonLoader.vue'
import { useRoute, RouterLink ,useRouter} from 'vue-router'
import BackButton from '@/components/BackButton.vue'
import { useToast } from 'vue-toastification'

const router = useRouter()
const toast = useToast()
const route = useRoute()
const jobId = route.params.id

const state = reactive({
  job: {},
  isLoading: true,
})

const deleteJob = async () =>{
  const confirm = window.confirm("Are you sure to delete " + state.job.title + " ?" )
  try{
    if(confirm){
      await axios.delete(`/api/jobs/${jobId}`)
      toast.success("Delete successfully")
      router.push('/jobs')
    }
  }
  catch(error){
    toast.error(error)
  }
}

onMounted(async () => {
  try {
    const response = await axios.get(`/api/jobs/${jobId}`)
    state.job = response.data
  } catch (error) {
    console.error('Failed to load job:', error)
  } finally {
    state.isLoading = false
  }
})


</script>

<template>

  <BackButton/>
  <!-- Job Content -->
  <section v-if="!state.isLoading" class="bg-green-50 min-h-screen">
    <div class="container m-auto py-10 px-6">
      <div class="grid grid-cols-1 md:grid-cols-[70%_30%] w-full gap-6">
        <!-- Main -->
        <main>
          <div class="bg-white p-6 rounded-lg shadow-md text-center md:text-left">
            <div class="text-gray-500 mb-4">{{ state.job.type }}</div>

            <h1 class="text-3xl font-bold mb-4">{{ state.job.title }}</h1>

            <div
              class="text-gray-500 mb-4 flex items-center justify-center md:justify-start"
            >
              <i class="pi pi-map-marker text-lg text-orange-700 mr-2"></i>
              <p class="text-orange-700">{{ state.job.location }}</p>
            </div>
          </div>

          <div class="bg-white p-6 rounded-lg shadow-md mt-6">
            <h3 class="text-green-800 text-lg font-bold mb-6">Job Description</h3>

            <p class="mb-4">
              {{ state.job.description }}
            </p>

            <h3 class="text-green-800 text-lg font-bold mb-2">Salary</h3>
            <p class="mb-4">{{ state.job.salary }}</p>
          </div>
        </main>

        <!-- Sidebar -->
        <aside>
          <div class="bg-white p-6 rounded-lg shadow-md">
            <h3 class="text-xl font-bold mb-6">Company Info</h3>

            <h2 class="text-2xl">
              {{ state.job.company?.name }}
            </h2>

            <p class="my-2">
              {{ state.job.company?.description }}
            </p>

            <hr class="my-4" />

            <h3 class="text-xl">Contact Email:</h3>
            <p class="my-2 bg-green-100 p-2 font-bold">
              {{ state.job.company?.contactEmail }}
            </p>

            <h3 class="text-xl">Contact Phone:</h3>
            <p class="my-2 bg-green-100 p-2 font-bold">
              {{ state.job.company?.contactPhone }}
            </p>
          </div>

          <!-- Manage Job -->
          <div class="bg-white p-6 rounded-lg shadow-md mt-6">
            <h3 class="text-xl font-bold mb-6">Manage Job</h3>

            <RouterLink
              :to="`/jobs/edit/${jobId}`"
              class="bg-green-500 hover:bg-green-600 text-white text-center font-bold py-2 px-4 rounded-full w-full block mt-4"
            >
              Edit Job
            </RouterLink>

            <button
              @click="deleteJob"
              class="bg-red-500 hover:bg-red-600 text-white font-bold py-2 px-4 rounded-full w-full mt-4 block"
            >
              Delete Job
            </button>
          </div>
        </aside>
      </div>
    </div>
  </section>
    <!-- Loading Spinner -->
  <div
    v-else="state.isLoading"
    class="absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2"
  >
    <MoonLoader color="#16a34a" size="60px" />
  </div>
</template>
