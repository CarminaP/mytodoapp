<template>
<div class="container">
    <h1>My ToDo App</h1>
    <div class="row">
      <form @submit.prevent="addTask">
        <div class="col s6">
          <input v-model="newTaskName" type="text">
        </div>

        <div class="col s6">
            <input type="submit" value="Agregar tarea" class="btn-large waves-effect waves-light blue btn">
        </div>
  </form>
    </div>
  

  <tasks-list :title="'Pendientes'" :task-list="tasksPending" @completar="toggleTask"/>

  <tasks-list :title="'Completados'" :task-list="tasksDone" @completar="toggleTask"/>

</div>

</template>

<script>
import TasksList from './components/TasksList'
import Firebase from 'firebase'

let config = {
    apiKey: "...",
    authDomain: "...",
    databaseURL: "...",
    storageBucket: "...",
    messagingSenderId: "..."
  };

let app = Firebase.initializeApp(config)
let db = app.database()
let tasksRef = db.ref('tasks')

export default{
  components: {
    TasksList
  },
  firebase: {
    tasks: tasksRef
  },
  data () {
    return {
      newTaskName: '',
      tasks: [
        {
          name: 'tarea 1',
          done: true
        }
      ]
    }
  },
  methods: {
    toggleTask (task) {
      tasksRef.child(task['.key']).set({
        name: task.name,
        done: !task.done
      })
      return task.name
    },
    addTask () {
    if(!this.newTaskName) return
     tasksRef.push({
      name: this.newTaskName,
      done: false
     })
     this.newTaskName = ''
    }
  },
  computed:{
    tasksPending (){
      return this.tasks.filter(task => !task.done)
    },
    tasksDone (){
      return this.tasks.filter(task => task.done)
    }
  }
}  
</script>
