<template>
  <div class="container">
    <div class="row">
      <div class="col-md-12">
        <h1>Kanban App ~ Ade Anugerah</h1>
        <hr>
        <div class="col-md-offset-10 col-md-1">
          <p align="right">
            <button type="submit" class="btn btn-primary" data-toggle="modal" data-target="#myModal">+ Add</button>
            <hr>
          </p>
        </div>
        <div class="col-md-3">
          <div class="row">
            <div class="col-sm-12">
              <div class="panel panel-primary">
                <div class="panel-heading">
                  <h3 class="panel-title">BackLog</h3>
                </div>
                <div class="panel panel-primary" v-for="backlog in backlogs" v-if="backlog.status == 'backlog' ">
                  <div class="panel-body">
                    <h3><b>{{ backlog.title }}</b></h3>
                    <p>Point: {{ backlog.point }}</p>
                    <p>Assigned To: {{ backlog.assignTo }}</p>
                  </div>
                  <div class="panel-footer">
                    <button @click="showDetail(backlog)" type="button" class="btn btn-info" data-toggle="modal" data-target="#modalDetail">Show Detail</button>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
        <div class="col-md-3">
          <div class="row">
            <div class="col-sm-12">
              <div class="panel panel-danger">
                <div class="panel-heading">
                  <h3 class="panel-title">To-Do</h3>
                </div>
                <div class="panel panel-primary" v-for="backlog in backlogs" v-if="backlog.status == 'todo' ">
                  <div class="panel-body">
                    <h3><b>{{ backlog.title }}</b></h3>
                    <p>Point: {{ backlog.point }}</p>
                    <p>Assigned To: {{ backlog.assignTo }}</p>
                  </div>
                  <div class="panel-footer">
                    <button @click="showDetail(backlog)" type="button" class="btn btn-info" data-toggle="modal" data-target="#modalDetail">Show Detail</button>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
        <div class="col-md-3">
          <div class="row">
            <div class="col-sm-12">
              <div class="panel panel-warning">
                <div class="panel-heading">
                  <h3 class="panel-title">Doing</h3>
                </div>
                <div class="panel panel-primary" v-for="backlog in backlogs" v-if="backlog.status == 'doing' ">
                  <div class="panel-body">
                    <h3><b>{{ backlog.title }}</b></h3>
                    <p>Point: {{ backlog.point }}</p>
                    <p>Assigned To: {{ backlog.assignTo }}</p>
                  </div>
                  <div class="panel-footer">
                    <button @click="showDetail(backlog)" type="button" class="btn btn-info" data-toggle="modal" data-target="#modalDetail">Show Detail</button>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
        <div class="col-md-3">
          <div class="row">
            <div class="col-sm-12">
              <div class="panel panel-success">
                <div class="panel-heading">
                  <h3 class="panel-title">Done</h3>
                </div>
                <div class="panel panel-primary" v-for="backlog in backlogs" v-if="backlog.status == 'done' ">
                  <div class="panel-body">
                    <h3><b>{{ backlog.title }}</b></h3>
                    <p>Point: {{ backlog.point }}</p>
                    <p>Assigned To: {{ backlog.assignTo }}</p>
                  </div>
                  <div class="panel-footer">
                    <button @click="showDetail(backlog)" type="button" class="btn btn-info" data-toggle="modal" data-target="#modalDetail">Show Detail</button>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    
    <div class="modal" id="myModal" role="dialog">
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <button type="button" class="close" data-dismiss="modal" aria-hidden="true">&times;</button>
            <h4 class="modal-title">Modal Kanban</h4>
          </div>
          <div class="modal-body">
            <form>
              <div class="form-group">
                <p align="left">Title:</p>
                <input v-model="addTasks.title" type="text" class="form-control">
              </div>
              <div class="form-group">
                <p align="left">Description:</p>
                <textarea v-model="addTasks.description" class="form-control" rows="5"></textarea>
              </div>
              <div class="form-group">
                <p align="left">Point:</p>
                <input v-model="addTasks.point" type="text" class="form-control">
              </div>
              <div class="form-group">
                <p align="left">Assign To:</p>
                <input v-model="addTasks.assignTo" type="text" class="form-control">
              </div>
            </form>
          </div>
          <div class="modal-footer">
            <button type="button" class="btn btn-default" data-dismiss="modal">Close</button>
            <button @click="addTask()" type="submit" class="btn btn-primary" data-dismiss="modal">Add Task</button>
          </div>
        </div>
      </div>
    </div>  
    
    <div v-if="currentTask !== null" class="modal" id="modalDetail" role="dialog">
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <button type="button" class="close" data-dismiss="modal" aria-hidden="true">&times;</button>
            <h4 class="modal-title">Detail Task : {{ currentTask.title }} for {{ currentTask.assignTo }}</h4>
          </div>
          <div class="modal-body">
            <form>
              <div class="form-group">
                <label align="left">Task Description: {{ currentTask.description }}</label>
                <p>Point: {{ currentTask.point }}</p>
                <p>Status: {{ currentTask.status }}</p>
              </div>
            </form>
          </div>
          <div class="modal-footer">
            <button v-if="currentTask.status == 'todo'" @click="backlog(currentTask)" type="button" class="btn btn-Primary" data-dismiss="modal">Backlog</button>
            <button @click="removeTask(currentTask)" type="button" class="btn btn-danger" data-dismiss="modal">Delete</button>
            <button v-if="currentTask.status == 'doing' || currentTask.status == 'backlog'" @click="todo(currentTask)" type="button" class="btn btn-primary" data-dismiss="modal">To-Do</button>
            <button v-if="currentTask.status == 'todo' || currentTask.status == 'done'" @click="doing(currentTask)" type="button" class="btn btn-warning" data-dismiss="modal">Doing</button>
            <button v-if="currentTask.status == 'doing'" @click="done(currentTask)" type="button" class="btn btn-success" data-dismiss="modal">Done</button>
          </div>
        </div>
      </div>
    </div>
    
  </div>
</template>

<script>
import firebase from 'firebase'

var config = {
  apiKey: 'AIzaSyBKbFE19ndQmxwM-nHxBCMcxCoLcmPzLek',
  authDomain: 'kanban-project-6cae8.firebaseapp.com',
  databaseURL: 'https://kanban-project-6cae8.firebaseio.com',
  projectId: 'kanban-project-6cae8',
  storageBucket: '',
  messagingSenderId: '861324455942'
}
var firebaseApp = firebase.initializeApp(config)
var db = firebaseApp.database().ref('kanban-task')
export default {
  data () {
    return {
      detailTaskModal: false,
      addTasks: {
        title: '',
        description: '',
        point: '',
        assignTo: '',
        status: 'backlog'
      },
      currentTask: null
    }
  },
  firebase: {
    backlogs: db
  },
  methods: {
    addTask () {
      db.push(this.addTasks)
      this.addTasks.title = ''
      this.addTasks.description = ''
      this.addTasks.point = ''
      this.addTasks.assignTo = ''
    },
    showDetail (backlog) {
      this.currentTask = backlog
    },
    removeTask (task) {
      db.child(task['.key']).remove()
    },
    backlog (tasks) {
      var data = db.child(tasks['.key'])
      data.update({
        'status': 'backlog'
      })
    },
    todo (tasks) {
      var data = db.child(tasks['.key'])
      data.update({
        'status': 'todo'
      })
    },
    doing (tasks) {
      var data = db.child(tasks['.key'])
      data.update({
        'status': 'doing'
      })
    },
    done (tasks) {
      var data = db.child(tasks['.key'])
      data.update({
        'status': 'done'
      })
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
