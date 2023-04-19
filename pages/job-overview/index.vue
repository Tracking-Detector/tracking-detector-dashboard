<template>
  <div>
    <v-card v-for="job in jobs" :key="job.id" class="mt-3">
      <v-card-title>{{ job.jobName }}</v-card-title>
      <v-card-subtitle>
        {{ job.jobDescription }} <br> {{ job.cronPattern }} <br> <v-chip small class="mt-2" :color="job.enabled ? 'green' : 'red'">
          {{ job.enabled ? "Enabled" : "Disabled" }}
        </v-chip>
      </v-card-subtitle>
      <v-card-actions>
        <v-btn
          @click="toggleJobById(job.id)"
        >
          Toggle
        </v-btn>
        <v-btn
          @click="triggerJobById(job.id)"
        >
          Trigger
        </v-btn>
        <v-btn
          :href="'/job-overview/'+job.id+'/runs'"
        >
          Runs
        </v-btn>
      </v-card-actions>
    </v-card>
  </div>
</template>
<script>
export default {
  async asyncData ({ $axios }) {
    const jobs = await $axios.$get('http://localhost:8000/tracking-detector/jobs')
    return { jobs }
  },
  methods: {
    toggleJobById (jobId) {
      this.$axios.$patch('http://localhost:8000/tracking-detector/jobs/' + jobId + '/toggle')
      window.location.reload()
    },
    triggerJobById (jobId) {
      this.$axios.$post('http://localhost:8000/tracking-detector/jobs/' + jobId + '/trigger')
      window.location.reload()
    }
  }
}
</script>
