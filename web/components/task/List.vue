<template>
  <div class="mx-4">
    <div class="flex items-center justify-between">
      <h1>
        {{ title }}
      </h1>
      <div>
        <FormSelect name="taskStatus" :value="currentFilterValue" :options="TASK_STATUS" @input="onSelectInput" />
      </div>
    </div>
    <hr class="my-4">
    <div class="flex flex-col rounded-lg shadow divide-y-0 md:divide-y-2 text-gray-500">
      <div v-for="task in filteredTasks" :key="task.id" class="p-4 md:bordered relative">
        <div class="flex items-center justify-between">
          <div class="flex items-center">
            <span :class="statusClass(task.status.value)" class="mr-2 px-2 rounded-full text-sm font-semibold">{{ task.status.label }}</span>
            <h2 class="text-lg text-indigo-600">
              {{ task.title }}
            </h2>
          </div>
          <div class="flex gap-2">
            <button class="bg-indigo-500 hover:opacity-80 text-white rounded">
              <SvgIcon name="pencil" class="w-5 h-5 m-1" />
            </button>
            <button class="bg-red-500 hover:opacity-80 text-white rounded">
              <SvgIcon name="trash" class="w-5 h-5 m-1" />
            </button>
          </div>
        </div>
        <div v-if="task.date" class="flex text-sm md:absolute md:right-0 mr-4">
          <SvgIcon name="calendar" class="w-5 h-5 m-1" />
          <span class="flex self-center pl-2">{{ formatDate(task.date) }}</span>
        </div>
        <p class="text-wrap">
          {{ task.description }}
        </p>
        <div class="flex flex-col md:flex-row gap-x-2 gap-y-1 text-sm mt-2">
          <p>
            Created at {{ formatDateTime(task.createdAt) }}
          </p>
          <strong>
            Updated at {{ formatDateTime(task.updatedAt) }}
          </strong>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { TASK_STATUS } from '~/dewib/api/tasks/index.js'

export default {
  props: {
    tasks: {
      type: Array,
      required: true
    },
    title: {
      type: String,
      default: 'My tasks'
    }
  },
  data () {
    return {
      TASK_STATUS,
      currentFilterValue: 'ALL'
    }
  },
  computed: {
    filteredTasks () {
      return this.currentFilterValue !== 'ALL' ? this.tasks.filter(t => t.status.value === this.currentFilterValue) : this.tasks
    }
  },
  methods: {
    statusClass (status) {
      switch (status) {
        case 'DONE':
          return 'bg-green-100 text-green-800'
        case 'TODO':
          return 'bg-red-100 text-red-800'
        default:
          return 'bg-yellow-100 text-yellow-800'
      }
    },
    formatDate (date) {
      return new Date(date).toLocaleDateString('en-US', {
        year: 'numeric',
        month: 'long',
        day: 'numeric'
      }).replace('at', '-')
    },
    formatDateTime (date) {
      return new Date(date).toLocaleTimeString('en-US', {
        year: 'numeric',
        month: 'long',
        day: 'numeric',
        hour12: false
      }).replace('at', '-')
    },
    onSelectInput (value) {
      this.currentFilterValue = value
    }
  }
}
</script>
