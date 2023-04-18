<template>
    <div>
        <h2>{{ job.jobName }} Runs: </h2>
        <v-card v-for="run in runs" :key="run.id" class="mt-3">
            <v-card-title>{{ run.id }}</v-card-title>
            <v-card-subtitle>StartDate: {{ run.startDate }} <br> EndDate {{ run.endDate }} <br> <v-chip small class="mt-2" > {{ run.status }}</v-chip> </v-card-subtitle>
            <v-expansion-panels>
                <v-expansion-panel>
                <v-expansion-panel-header>
                    Logs
                </v-expansion-panel-header>
                <v-expansion-panel-content>
                    <v-textarea
                    filled
                    auto-grow
                    rows="4"
                    row-height="30"
                    shaped
                    readonly
                    disabled
                    :value="run.logs"
                    ></v-textarea>
                </v-expansion-panel-content>
                </v-expansion-panel>
            </v-expansion-panels>
        </v-card>
    </div>
</template>
<script>
export default {
  async asyncData ({ params, $axios }) {
    const runs = await $axios.$get('http://localhost:8000/tracking-detector/jobs/' + params.jobId + '/runs')
    const job = await $axios.$get('http://localhost:8000/tracking-detector/jobs/' + params.jobId)
    return { runs, job }
  }
}
</script>
