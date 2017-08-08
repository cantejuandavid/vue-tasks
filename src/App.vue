<template lang="pug">  
  
  #app.row
    div.col-md-8.col-md-offset-2.panel.panel-default
      div.col-md-12
        img(src="./assets/tasks-banner.jpg", alt="Tasks!").img-responsive
      div.col-md-6
        h1 {{name}}
        form(v-on:submit.prevent="addTask").form-horizontal
          h2 add tasks
          .form-group
            label.col-sm-2.control-label Title
            div.col-sm-10
              input(type="text" v-model="newTask.title" placeholder="title").form-control
          .form-group
            label.col-sm-2.control-label Time
            div.col-sm-10
              input(type="text" v-model="newTask.time" placeholder="time").form-control
          .form-group
            .col-sm-offset-2.col-sm-10
              button.btn.btn-primary Add Task
              button(type="button" v-on:click="cancel").btn.btn-danger Cancel
      div.col-md-6
        h2 Task Lists
        p.bg-success Horas totales trabajadas {{ totalTime }}
        ul(style="list-style:none", v-if="tasks.length")
          li(v-for="(task, index) in tasks")
            div                            
              button(type="button", v-on:click="removeTask(index)").btn.btn-danger.btn-sm <strong> X</strong>
              span   {{ task.title }} Time: {{ task.time }}
        p(v-else) No hay tareas
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      name: 'App tasks!',
      tasks: [],
      newTask: {
        title: '',
        time: 0
      }
    }
  },
  created: function () {
    this.tasks = JSON.parse(localStorage.getItem('tasks')) || []
  },
  methods: {
    addTask: function () {
      if (this.newTask.title && this.newTask.title && !isNaN(this.newTask.time)) {
        let newTaskObj = {
          title: this.newTask.title,
          time: this.newTask.time
        }
        this.tasks.push(newTaskObj)
        localStorage.setItem('tasks', JSON.stringify(this.tasks))
        this.newTask = {}
      } else {
        alert('Empty fields or Time is not a number')
      }
    },
    removeTask: function (index) {
      console.log(index)
      this.tasks.splice(index, 1)
      localStorage.setItem('tasks', JSON.stringify(this.tasks))
    },
    cancel: function () {
      this.newTask = {}
    }
  },
  computed: {
    totalTime: function () {
      let time = 0
      this.tasks.forEach(function (task) {
        time += parseInt(task.time)
      })
      return time
    }
  }
}
</script>

<style lang="stylus" scoped>

</style>