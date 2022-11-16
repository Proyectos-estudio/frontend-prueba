<template>
  <div class="w-full mx-20 mt-32">
    <div class="w-full overflow-x-auto relative shadow-md sm:rounded-lg">
      <h2 class="w-full font-bold text-xxl text-blue-600">Listado de tareas</h2>
      <table class="w-full text-sm text-left text-gray-500 dark:text-gray-400">
        <thead
          class="text-xs text-gray-700 uppercase bg-gray-50 dark:bg-gray-700 dark:text-gray-400"
        >
          <tr>
            <th scope="col" class="py-3 px-6">Titulo</th>
            <th scope="col" class="py-3 px-6">Descripción</th>
            <!-- <th scope="col" class="py-3 px-6">Archivos</th> -->
            <th scope="col" class="py-3 px-6">Completada</th>
            <th scope="col" class="py-3 px-6">Action</th>
          </tr>
        </thead>
        <tbody>
          <tr
            v-for="(task, index) in tasks"
            :key="index"
            class="bg-white border-b dark:bg-gray-900 dark:border-gray-700"
          >
            <th
              scope="row"
              class="py-4 px-6 font-medium text-gray-900 whitespace-nowrap dark:text-white"
            >
              {{ task.title }}
            </th>
            <td class="py-4 px-6">{{ task.description }}</td>
            <!-- <td class="py-4 px-6">{{task.files}}</td> -->
            <td class="py-4 px-6">
              {{ task.completed === 0 ? 'No completada' : 'Completada' }}
            </td>
            <td class="py-4 px-6">
              <a
                href="#"
                class="font-medium text-blue-600 dark:text-blue-500 hover:underline"
                >Edit</a
              >
              |
              <button
                class="font-medium text-blue-600 dark:text-blue-500 hover:underline"
                @click="deleteTask(task.id)"
                >
                Delete</button
              >
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
export default {
  name: 'TasksPage',
  layout: 'Tasks',
  data() {
    return {
      files: [],
      tasks: [],
    }
  },

  async mounted() {
    await this.InitFunctions()
  },

  methods: {
    async InitFunctions() {
      await this.getTasks()
    },
    async getTasks() {
      const response = await this.$axios.get('api/tasks')
      this.tasks = response.data
    },
    async deleteTask(id) {
      await this.$axios.delete(`api/tasks/${id}`)
      this.getTasks();
      
    },
  },
}
</script>

<style></style>
