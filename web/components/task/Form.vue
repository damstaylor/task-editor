<template>
  <div class="flex flex-col items-center">
    <div class="sm:w-96">
      <div class="flex flex-col items-center gap-4 my-4">
        <SvgIcon :name="getIconName" class="w-10 h-10 text-gray-700" />
        <h1 class="text-center text-2xl font-bold mb-4">
          {{ getTitle }}
        </h1>
      </div>
      <form class="px-8 py-6 rounded-lg shadow" @submit.prevent="handleSubmit">
        <div class="mb-4">
          <FormInput label="Title" name="title" v-model="taskFormData.title" type="text" />
        </div>
        <div class="mb-4">
          <FormInputTextArea label="Describe it" name="description" v-model="taskFormData.description" />
        </div>
        <div class="flex gap-2 mb-4">
          <div class="flex-1 w-1/2">
            <FormInput label="Date" name="date" v-model="taskFormData.date" type="date" />
          </div>
          <div class="flex-1 w-1/2">
            <FormSelect
              name="taskStatus-form"
              label="Status"
              :value="taskFormData.status"
              :options="TASK_STATUS"
              @input="onSelectInput"
            />
          </div>
        </div>
        <div class="flex flex-col gap-4">
          <button type="submit" class="bg-indigo-500 text-white py-2 px-4 rounded">
            Submit
          </button>
          <button type="button" class="bg-gray-100 text-gray-700 py-2 px-4 rounded" @click="navigateHome()">
            Cancel
          </button>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
const TASK_STATUS = [
  {
    label: 'To Do',
    value: 'TODO'
  },
  {
    label: 'Done',
    value: 'DONE'
  }
]

export default {
  props: {
    title: {
      type: String,
      default: ''
    },
    id: {
      type: String,
      default: ''
    }
  },
  data () {
    return {
      TASK_STATUS,
      taskFormData: {
        title: '',
        description: '',
        date: '',
        status: 'TODO'
      }
    }
  },
  computed: {
    isUpdate () {
      return !!this.id
    },
    getTitle () {
      return this.isUpdate ? 'Edit a task' : 'Add a task'
    },
    getIconName () {
      return this.isUpdate ? 'pencil' : 'plus-circle'
    }
  },
  async mounted () {
    if (this.isUpdate) {
      const [err, task] = await this.$api.tasks.findOne(this.id)
      if (!err) {
        const taskFormData = { ...task, status: task.status.value }
        if (taskFormData.date) {
          taskFormData.date = task.date.slice(0, 10)
        }
        this.taskFormData = taskFormData
      }
    }
  },
  methods: {
    async handleSubmit () {
      const body = { ...this.taskFormData }
      if (this.taskFormData.date) {
        body.date = new Date(this.taskFormData.date).toISOString()
      } else {
        delete body.date
      }
      if (this.isUpdate) {
        await this.$api.tasks.update(this.id, body)
      } else {
        await this.$api.tasks.create(body)
      }
      this.navigateHome()
    },
    navigateHome () {
      this.$router.push('/')
    }
  }
}
</script>
