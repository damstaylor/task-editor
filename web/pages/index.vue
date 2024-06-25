<template>
  <div class="m-10 container mx-auto">
    <p v-if="$fetchState.pending">
      Fetching tasks...
    </p>
    <p v-else-if="$fetchState.error" class="text-red-700 text-2xl py-5">
      Unable to fetch tasks.
    </p>
    <TaskList v-else :tasks="tasks" />
  </div>
</template>

<script>
export default {
  data () {
    return {
      tasks: []
    }
  },
  async fetch () {
    const [err, tasks] = await this.$api.tasks.findAll()

    if (err) {
      throw new Error(err)
    }

    this.tasks = tasks
  }
}
</script>
