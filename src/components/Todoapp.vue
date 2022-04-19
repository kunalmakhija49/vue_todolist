<template>
  <div class="container">
    <h2 class="text-center mt-5">To Do List</h2>
    <form v-on:submit.prevent="submitTask">
    <div class="d-flex">
      
      <input
        v-model="task"
        type="text"
        placeholder="Enter Task"
        class="form-control"
        name="task"
      />
      <button :disabled="task ===''" type="submit" class="btn btn-success rounded-30">
        Add
      </button>
      
    </div>
    </form>
    <table class="table table-bordered mt-5">
      <thead>
        <tr>
          <th scope="col">Task</th>
          <th scope="col">Status</th>
          <th scope="col" class="text-center"></th>
          <th scope="col" class="text-center"></th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(task, index) in list" v-bind:key="task.id" >
          <td>
            <span :class="{ finished: task.Status === 'finished' }">{{
              task.task
            }}</span>
          </td>
          <td style="width: 120px">
            <span
              @click="changeStatus(index)"
              class="pointer noselect"
              :class="{
                'text-danger': task.status === 'to-do',
                'text-success': task.status === 'finished',
                'text-warning': task.status === 'in-progress',
              }"
            >
              {{task.status}}</span
            >
          </td>
          <td>
            <div class="text-center">
              <button class="btn btn-warning" @click="editTask(index)">
                Edit
              </button>
            </div>
          </td>
          <td>
            <button class="btn btn-danger" @click="deleteTask(task)">
              Delete
            </button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
// import Vue from 'vue';
import axios from "axios";
// import VueAxios from 'vue-axios'
export default {
  name: "HelloWorld",
  props: {
    msg: String,
  },
  data() {
    return {
      task:"",
      editedTask: null,
      availableStatus: ["to-do", "in-progress", "finished"],
      list: undefined,
    };
  },
  mounted() {
    this.gettodos();
    this.submitTask();
  },
  watch: {},
  methods: {
    gettodos() {
      axios.get("http://127.0.0.1:8000/api/show").then((resp) => {
        this.list = resp.data;
        // console.log(resp.data);
      });
    },
    submitTask(e) {
      if(this.task==""){
        return;
      }

      axios.post('http://127.0.0.1:8000/api/create',{
        task: this.task
      }).then(response=>{
        if(response.status==201){
          this.task="";
          this.$emit('reloadlist');
          this.gettodos();
        }
      }).catch(error=>{
        console.log(error);
      })
      e.preventDefault();

    },
    deleteTask(task) {
      // this.tasks.splice(index, 1);

      // alert(index);
      var id = task.id;
      axios.post('http://127.0.0.1:8000/api/todo_delete/',{id:id})
      .then(response=>{
        if(response.status == 200){
          this.$emit('itemchanged')
          this.gettodos();
        }
      }).catch(error=>{
        console.log(error);
      })
      

    },
    editTask(index) {
      this.task = this.tasks[index].name;
      this.editedTask = index;
    },
    changeStatus(index) {
      let newIndex = this.availableStatus.indexOf(this.tasks[index].Status);
      if (++newIndex > 2) newIndex = 0;
      this.tasks[index].Status = this.availableStatus[newIndex];
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
* {
  /* background: rgb(66, 66, 66);
   */
  /* background:rgb(2, 92, 83); */
  background:aliceblue
}
.pointer {
  cursor: pointer;
}
.finished {
  text-decoration: line-through;
}
.noselect {
  user-select: none;
}
</style>
