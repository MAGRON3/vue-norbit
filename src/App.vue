<template>
  <div id="app">
    <h1>Corporate time tracking portal</h1>
    <hr/>
    <table v-if="!loader">
      <tr>
        <td><strong>Projects</strong></td>
        <td><strong>Tasks</strong></td>
        <td><strong>Wirings</strong></td>
      </tr>

      <tr>
        <td>
          <ProjectCreateForm
            @add-project="AddProject"
          />
        </td>
        <td>
          <TaskCreateForm
            v-bind:current_projects="FilterProjects"
            @add-task="AddTask"
          />
        </td>
        <td>
          <WiringCreateForm
            v-bind:current_tasks="FilterTasks"
            v-bind:current_wirings="current_wirings"
            @add-wiring="AddWiring"
          />
        </td>
      </tr>

      <tr>
        <td>
          <ProjectFilter
            @apply-filter="ApplyFilterProjects"
          />
        </td>
        <td>
          <TaskFilter
            @apply-filter="ApplyFilterTasks"
          />
        </td>
        <td>
          <WiringFilter
            @apply-filter="ApplyFilterWirings"
          />
        </td>
      </tr>

      <tr>
        <td>
          <div>
            <ProjectsList 
              v-bind:current_projects="FilterProjects"
              @delete-project="DeleteProject"
              @change-project="ChangeProject"
            />
          </div>
        </td>
        <td>
          <div>
          <TaskList
            v-bind:current_tasks="FilterTasks"
            v-bind:current_projects="current_projects"
            @delete-task="DeleteTask"
            @change-task="ChangeTask"
          />
          </div>
        </td>
        <td>
          <div>
          <WiringList
            v-bind:current_wirings="FilterWirings"
            v-bind:current_tasks="current_tasks"
            @delete-wiring="DeleteWiring"
            @change-wiring="ChangeWiring"
          />
          </div>
        </td>
      </tr>
    </table>
    <div v-else> Loading...
    </div>
    <hr/>
  </div>
</template>

<script>
import ProjectsList from '@/components/Lists/ProjectsList.vue'
import TaskList from '@/components/Lists/TaskList.vue'
import WiringList from '@/components/Lists/WiringList.vue'

import ProjectCreateForm from '@/components/CreateForms/ProjectCreateForm.vue'
import TaskCreateForm from '@/components/CreateForms/TaskCreateForm.vue'
import WiringCreateForm from '@/components/CreateForms/WiringCreateForm.vue'

import ProjectFilter from '@/components/Filters/ProjectsFilter.vue'
import TaskFilter from '@/components/Filters/TasksFilter.vue'
import WiringFilter from '@/components/Filters/WiringsFilter.vue'

export default {
  name: 'App',
  data(){
    return{
      current_projects:[],
      current_tasks:[],
      current_wirings:[],

      filter_projects:{id:'',name:'',active:''},
      filter_tasks:{id:'',name:'',active:''},
      filter_wirings:{taskName:'',name:'',date: ''},

      loader: true,
    }
  },
  
  components: {
    ProjectsList,
    TaskList,
    WiringList,
    ProjectCreateForm,
    TaskCreateForm,
    WiringCreateForm,
    ProjectFilter,
    TaskFilter,
    WiringFilter,
  },

  mounted() {
    fetch('https://localhost:5001/db/projects')
    .then(response => response.json())
    .then(json => {
      this.current_projects = json;
    });
    fetch('https://localhost:5001/db/tasks')
    .then(response => response.json())
    .then(json => {
      this.current_tasks = json;
    });
    fetch('https://localhost:5001/db/wirings')
    .then(response => response.json())
    .then(json => {
      this.current_wirings = json;
      this.loader = false;
    });
  },

  methods:{
    AddProject(newProject){
        const requestOptions = {
          method: "POST",
          headers: { "Content-Type": "application/json; charset=utf-8" },
          body: JSON.stringify({ name: newProject.name, p_code: newProject.p_code, active: newProject.active })
        };
        fetch("https://localhost:5001/db/projects", requestOptions)
        .then(response => response.json())
        .then(json => {
          newProject = json;
          this.current_projects.push(newProject);
        });
        
    },

    DeleteProject(id)
    {
      for(let i = 0; i < this.current_projects.length; i++){
        if (this.current_projects.at(i).id === id){
          for(let j = this.current_tasks.length - 1; j >= 0; j--)
          {
            if (this.current_tasks.at(j).project_code === this.current_projects.at(i).id)
            {
              this.DeleteTask(this.current_tasks.at(j).id);
            }
          }
          const requestOptions = {
            method: "DELETE",
          };
          console.log(requestOptions);
            fetch("https://localhost:5001/db/projects/"+this.current_projects.at(i).id, requestOptions)
            .then(response => response.json());
          this.current_projects.splice(i,1);
          break;
        }
      }
    },

    ChangeProject(id, projectActive, newIDForProject,newNameForProject){
      for(let i = 0; i < this.current_projects.length; i++){
        if (this.current_projects.at(i).id === id){
          const requestOptions = {
            method: "PUT",
            headers: { "Content-Type": "application/json; charset=utf-8" },
            body: JSON.stringify({id: this.current_projects.at(i).id, name: newNameForProject, p_code: newIDForProject, active: projectActive })
          };
          fetch("https://localhost:5001/db/projects", requestOptions)
          .then(response => response.json())
          .then(json => {
            this.current_projects.at(i).name = json.name;
            this.current_projects.at(i).p_code = json.p_code;
            this.current_projects.at(i).active = json.active;
          });
          break;
        }
      }
    },

    AddTask(newTask){
        const requestOptions = {
          method: "POST",
          headers: { "Content-Type": "application/json; charset=utf-8" },
          body: JSON.stringify({ name: newTask.name, project_code: newTask.project_code, active: newTask.active })
        };
        fetch("https://localhost:5001/db/tasks", requestOptions)
        .then(response => response.json())
        .then(json => {
          newTask = json;
          this.current_tasks.push(newTask);
        });
    },

    DeleteTask(id)
    {
      for(let i = 0; i < this.current_tasks.length; i++){
        if (this.current_tasks.at(i).id === id){
          for(let j = this.current_wirings.length - 1; j >= 0; j--)
          {
            if (this.current_wirings.at(j).task_code === this.current_tasks.at(i).id)
            {
              this.DeleteWiring(this.current_wirings.at(j).id);
            }
          }
          const requestOptions = {
            method: "DELETE",
          };
          console.log(requestOptions);
            fetch("https://localhost:5001/db/tasks/"+this.current_tasks.at(i).id, requestOptions)
            .then(response => response.json());
          this.current_tasks.splice(i,1);
          break;
        }
      }
    },

    ChangeTask(id,taskActive,newIDForTask,newNameForTask){
      for(let i = 0; i < this.current_tasks.length; i++){
        if (this.current_tasks.at(i).id === id){
          const requestOptions = {
            method: "PUT",
            headers: { "Content-Type": "application/json; charset=utf-8" },
            body: JSON.stringify({ id: this.current_tasks.at(i).id, name: newNameForTask, project_code: newIDForTask, active: taskActive })
          };
          fetch("https://localhost:5001/db/tasks", requestOptions)
          .then(response => response.json())
          .then(json => {
            this.current_tasks.at(i).name = json.name;
            this.current_tasks.at(i).project_code = json.project_code;
            this.current_tasks.at(i).active = json.active;
          });
          break;
        }
      }
    },

    AddWiring(newWiring){
      const requestOptions = {
          method: "POST",
          headers: { "Content-Type": "application/json; charset=utf-8" },
          body: JSON.stringify({ name: newWiring.name, task_code: newWiring.task_code, w_date: newWiring.w_date, w_hours: newWiring.w_hours })
        };
        fetch("https://localhost:5001/db/wirings", requestOptions)
        .then(response => response.json())
        .then(json => {
          newWiring = json;
          this.current_wirings.push(newWiring);
        });
    },


    DeleteWiring(id)
    {
      for(let i = 0; i < this.current_wirings.length; i++){
        if (this.current_wirings.at(i).id === id){
          const requestOptions = {
            method: "DELETE",
          };
          console.log(requestOptions);
            fetch("https://localhost:5001/db/wirings/"+this.current_wirings.at(i).id, requestOptions)
            .then(response => response.json());
          this.current_wirings.splice(i,1);
          break;
        }
      }
    },

    ChangeWiring(id,newIDTask,inputNewWiringTaskHours,inputNewDate,inputNewWiringName){
      for(let i = 0; i < this.current_wirings.length; i++){
        if (this.current_wirings.at(i).id === id){
          const requestOptions = {
            method: "PUT",
            headers: { "Content-Type": "application/json; charset=utf-8" },
            body: JSON.stringify({ id: this.current_wirings.at(i).id,  name: inputNewWiringName, task_code: newIDTask, w_date: inputNewDate, w_hours: inputNewWiringTaskHours })
          };
          fetch("https://localhost:5001/db/wirings", requestOptions)
          .then(response => response.json())
          .then(json => {
            this.current_wirings.at(i).name = json.name;
            this.current_wirings.at(i).w_hours = json.w_hours;
            this.current_wirings.at(i).task_code = json.task_code;
            this.current_wirings.at(i).w_date = json.w_date;
          });

          break;
        }
      }
    },

    ApplyFilterProjects(pID, pName, pActive){
      this.filter_projects.id = pID;
      this.filter_projects.name = pName;
      this.filter_projects.active = pActive;
    },

    ApplyFilterTasks(pID, pName, pActive){
      this.filter_tasks.id = pID;
      this.filter_tasks.name = pName;
      this.filter_tasks.active = pActive;
    },

    ApplyFilterWirings(pTaskName, pName, pDate){
      this.filter_wirings.taskName = pTaskName;
      this.filter_wirings.name = pName;
      this.filter_wirings.date = pDate;
    },

    GetProjectCodeForId(projectID)
    {
      for(let i = 0; i < this.current_projects.length; i++)
      {
        if (this.current_projects.at(i).id === projectID) return this.current_projects.at(i).p_code.toString();
      }
      return '';
    }
  },

  computed:{
    FilterProjects() {
      if (this.filter_projects.active === 'all') return this.current_projects.filter(t => t.name.indexOf(this.filter_projects.name) > -1 && t.p_code.toString().indexOf(this.filter_projects.id) > -1 );
      if (this.filter_projects.active === 'active') return this.current_projects.filter(t => t.active && t.name.indexOf(this.filter_projects.name) > -1 && t.p_code.toString().indexOf(this.filter_projects.id) > -1);
      if (this.filter_projects.active === 'inactive') return this.current_projects.filter(t => !t.active && t.name.indexOf(this.filter_projects.name) > -1 && t.p_code.toString().indexOf(this.filter_projects.id) > -1);
      return  this.current_projects;
    },

    FilterTasks() {
      if (this.filter_tasks.active === 'all') return this.current_tasks.filter(t => ((t.name.indexOf(this.filter_tasks.name) > -1) && this.GetProjectCodeForId(t.project_code).indexOf(this.filter_tasks.id) > -1));
      if (this.filter_tasks.active === 'active') return this.current_tasks.filter(t => (t.active && (t.name.indexOf(this.filter_tasks.name) > -1) && this.GetProjectCodeForId(t.project_code).indexOf(this.filter_tasks.id) > -1));
      if (this.filter_tasks.active === 'inactive') return this.current_tasks.filter(t => (!t.active && (t.name.indexOf(this.filter_tasks.name) > -1) && this.GetProjectCodeForId(t.project_code).indexOf(this.filter_tasks.id) > -1));
      return  this.current_tasks;
    },

    FilterWirings() {
      let filtered_for_name = this.current_wirings.filter(t => ((t.name.indexOf(this.filter_wirings.name)) > -1));
      let filtered_task_for_name = this.current_tasks.filter(t => ((t.name.indexOf(this.filter_wirings.taskName)) > -1));
      let filtered_result = new Array;
      for (let i = 0; i < filtered_for_name.length; i++){
        for (let j = 0; j < filtered_task_for_name.length; j++){
          if(filtered_for_name.at(i).task_code === filtered_task_for_name.at(j).id) {
            filtered_result.push(filtered_for_name.at(i));
            break;
          }
        }
      }
      return filtered_result.filter(t => t.w_date.indexOf(this.filter_wirings.date) > -1);
    },
  }
}
</script>

<style>
td{
 vertical-align:top;
}

.frame_div{
    border: 1px solid #ccc;
    justify-content: space-between;
    padding: .5rem 1rem;
    margin-bottom: 0rem;
    height:180px;
    width:600px;
    display: table-cell;
    vertical-align: text-top
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
