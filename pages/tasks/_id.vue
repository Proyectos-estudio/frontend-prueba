<template>
  <div class="w-full h-screen flex flex-col justify-center items-center">
    <h2 class="text-blue-500">Editar Tarea</h2>
    <div class="w-1/3 flex flex-col justify-center">
      <div class="mb-6">
        <label
          for="base-input"
          class="block mb-2 text-sm font-medium text-gray-900 dark:text-white"
          >Titulo</label
        >
        <input
          id="base-input"
          v-model="title"
          placeholder="Titulo de la tarea"
          type="text"
          class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
        />
      </div>
      <div>
        <label
          for="small-input"
          class="block mb-2 text-sm font-medium text-gray-900 dark:text-white"
          >Descripción</label
        >
        <textarea
          id="message"
          v-model="description"
          rows="4"
          class="block mb-5 p-2.5 w-full text-sm text-gray-900 bg-gray-50 rounded-lg border border-gray-300 focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
          placeholder="Escriba una descripción de la tarea"
        ></textarea>

        <label
          for="countries"
          class="block mb-2 text-sm font-medium text-gray-900 dark:text-white"
          >Estado de la tarea</label
        >
        <select
          id="countries"
          v-model="status"
          class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
        >
          <option value="0">Sin completar</option>
          <option value="1">Completada</option>
        </select>
      </div>
      <div class="w-full flex justify-center mt-6">
        <button
          type="submit"
          class="inline-flex items-center px-5 py-2.5 text-sm font-medium text-center text-white bg-blue-700 rounded-lg focus:ring-4 focus:ring-blue-200 dark:focus:ring-blue-900 hover:bg-blue-800"
          @click="updateTask()"
        >
          Editar tarea
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'EditTasksPage',
  layout: 'Tasks',

  data() {
    return {
      title: '',
      description: '',
      file: null,
      filesNames: [],
      status: 0,
    }
  },

  computed: {
    idTask() {
      return this.$route.params.id
    },
  },

  mounted() {
    this.getTask()
  },

  methods: {
    async getTask() {
      const response = await this.$axios.get(`api/tasks/${this.idTask}`)
      this.title = response.data.title
      this.description = response.data.description
      this.filesNames = response.data.files
    },
    async updateTask(){
        const payload = {
            title: this.title,
            description: this.description,
            status: this.status,
        }
        try {
            await this.$axios.put(`api/tasks/${this.idTask}`, payload)
            this.$notify({
                title: 'Tarea editada',
                message: 'La tarea se ha editado correctamente',
                type: 'primary',
                duration: 3000,
            })
            this.$router.push('/tasks')
        } catch (error) {
            
        }

    }
  },
}
</script>

<style></style>
