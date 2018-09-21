<template>
  <div>
    <h3>{{ title }}</h3>
      <ul>
        <li v-for="(task, index) in taskList"
            :key="index">
          {{ task.name }}
          <button class="btn-small green btn-floating" v-if="!task.done" type="button" @click="toggleTasks(task)"><i class="large material-icons">check</i></button>
        </li>
      </ul>
  </div>
</template>

<script>
export default {
  props: {
    title: {
      type: String,
      required: true
    },
    taskList: {
      type: Array,
      required: true
    }
  },
  methods: {
    toggleTasks (task) {
      this.$emit('completar', task)
    },
    deleteTask (task) {
      this.$emit('borrar', task)
    }
  }
}
</script>
