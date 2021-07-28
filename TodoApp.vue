<template>
<div class="container" style="max-width:600px">
   <h2 class="text-center mt-5">Vue TodoApp</h2>

   <div class="d-flex mt-5">
     <input type="text"  v-model="task" placeholder="Enter Your Task" 
     class="w-100 from-control"/>
     <button class="btn btn-warning rounded-0" @click="submitTask">
       Submit
     </button>
   </div>
    <div class="position-relative">
      <h5 class="text-center mt-5">
        <small class="text-muted">Have a Nice Day!!</small>
      </h5>
    </div>
   <!-- task Table-->
   <table class="table table-bordered mt-5">
     <thead>
       <tr>
         <th scope="col">Task</th>
         <th scope="col" style="width:120px">Status</th>
          <th scope="col" class="text-center">Edit</th>
          <th scope="col" class="text-center">Delete</th>
       </tr>
     </thead>
     <tbody>
       <tr  v-for="(task, index) in tasks" :key="index">
         <td>  <span :class="{ 'line-through': task.status === 'finished' }">
              {{ task.name }}
            </span>
         </td>
         <td> 
          <span              
           class="pointer noselect"
              @click="changeStatus(index)"
              :class="{
                'text-danger': task.status === 'to-do',
                'text-success': task.status === 'finished',
                'text-warning': task.status === 'in-progress',
              }"
            >
              {{task.status }}
          </span>
        </td>
         <td>
           <div class="text-center" @click="editTask(index)">
             <span class="fa fa-pen"></span>
           </div>
         </td>
         <td>
           <div class="text-center" @click="deleteTask(index)">
             <span class="fa fa-trash"></span>
           </div>
         </td>
       </tr>
     </tbody>

     <footer>
      <small><span class="fixed-bottom">@copyright-- SupratimSarkar,2021</span></small>
     </footer>
   </table>
</div>
</template>

<script>
export default {
  name: 'TodoApp',
  props: {
    msg: String
  },
  data(){
    return{
      task: "",
      editedTask:null,
      statuses: ["to-do", "in-progress", "finished"],
      tasks:[
        // {
        //   name:"Do your job by yourself",
        //   status:"To-do",
        // }
        //   {
        //   name:"Do your job by yourself--2",
        //   status:"in-progress",
        // },
        //   {
        //   name:"Do your job by yourself--3",
        //   status:"finished",
        // }
      ]
    }
  },
  methods:{
    submitTask(){
    // if there is no task
       if (this.task.length === 0) return;

    //if we need to update..
    if(this.editedTask != null){
      this.tasks[this.editedTask].name= this.task;
      this.editedTask=null;
    } 
    else{
      // need to update a new task..
      this.tasks.push({
        name:this.task,
        status:"todo",
      });
    } 
    
    this.task="";
    },
    //detele task..

    deleteTask(index) {
      this.tasks.splice(index, 1);
    }, 

    //edit task--> itv will assigen the task of index to the this.task and you will easily edit that from there..
    editTask(index){
      this.task=this.tasks[index].name;
      this.editedTask= index;
    },
    // each click increase the newIndex and if it is bigger then 2 set to deafult ie. 0
    changeStatus(index) {
      let newIndex = this.statuses.indexOf(this.tasks[index].status);
      if (++newIndex > 2) newIndex = 0;
      this.tasks[index].status = this.statuses[newIndex];
    },
  }
}
</script>
<style scoped>
.pointer {
  cursor: pointer;
}
.noselect {
  -webkit-touch-callout: none;
  -webkit-user-select: none;
  -khtml-user-select: none;
  -moz-user-select: none; 
  -ms-user-select: none;
  user-select: none; 
}
.line-through {
  text-decoration: line-through;
}

