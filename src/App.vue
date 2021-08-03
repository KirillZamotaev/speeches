<template>
  <div id="app" class="container-fluid text-center">
    <h1 class="text-info">{{ title }}</h1>
    <div class="col">
      <form @submit.prevent="addTask">
        <div class="input-group mb-3">
            <button class="btn" type="submit">
              Request quote
            </button>      
            <button class="btn btn-danger mb-3" @click.prevent="deleteMulti">Delete all</button>
        </div>
      </form>

      <ul class="list-group">
        <li v-for="(task_name,index) in tasks" :key="index" class="list-group-item list-group-item-info">
          <div class="row">
            <div class="col">
              <div class="task">
                <button class="btn btn-close" @click="deleteTask(task_name)">X</button>
                <div class="task-name">{{ task_name['task'] }}</div>
              </div>
            </div>
          </div>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      title: 'Kanye`s speeches',
      taskName: "",
      tasks: [],
      error: false,
      update: false,
      updateIndex: null,
      ids: [],
      deleteMultiple: false
    }
  },
  methods: {
    deleteMulti: function(){
      this.tasks = [];
    },
    deleteTask: function({ task: taskName }) {
        this.tasks = this.tasks.filter((task) => {
          return task['task'] !== taskName;
        });
    },
    changeCheck: function(e){
      this.tasks[e.target.id]['check'] = e.target.checked
      var count = 0
      this.tasks.forEach((task) => {
        if(task.check == true){
          count = count + 1
        }
      })
      this.deleteMultiple = (count > 0) ? true : false
      console.log("changeCheck");
      this.tasks.forEach((task) => {
        console.log(task.task, task.check);
      });
    },
    loadData: function(index){
      this.taskName = this.tasks[index]['task']
      this.updateIndex = index
      this.update=true
    },
    requestData: async function() {
      const res = await fetch('https://api.kanye.rest/');
      const data = await res.json(); 
      const theIndex = this.tasks.length;
      this.tasks.push({
            "index": theIndex,
            "task": data.quote,
            })
    },
    addTask: function(){
       this.requestData()
    },
  }
}
</script>

<style>
  .row, .col, .input-group, .list-group, .list-group-item, .task {
    display: flex;  
  }

  .btn {
    border: 1px solid #bbb;
    border-radius: 0.3rem;
    padding: 0.5rem;
  }

  .input-group > * {
    margin-right: 0.5rem;
  }

  .list-group {
    padding: 0;
    flex-direction: column;
  }

  .task {
    margin: 0.5rem 0;
    justify-content: space-between;
    align-items: center;
  }

  .col {
    flex-direction: column;
  }

  .btn-close {
    margin-right: 1.5rem;
  }

</style>