<template>
  <div id="app">
    <h1>Корпоративный портал учета рабочего времени</h1>
    <table>
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
    <hr/>
    <div>
      <strong>TODO List</strong>
      <ul>
        <li>1</li>
      </ul>
    </div>
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
      current_projects:[
        {id: "1", p_code: "1", name: 'Project 1 Name', active: true},
        {id: "2", p_code: "2", name: 'Project 2 Name', active: false},
        {id: "3", p_code: "3", name: 'Project 3 Name', active: true}
      ],
      current_tasks:[
        {id: "1", name: 'Task 1 Name', projectID: "1", active: true},
        {id: "2", name: 'Task 2 Name', projectID: "1", active: false},
        {id: "3", name: 'Task 3 Name', projectID: "1", active: true}
      ],
      current_wirings:[
        {id: "1", w_date: "2022-01-01", w_hours: 1, name: 'Wiring 1 Description', taskID: "1"},
        {id: "2", w_date: "2022-01-01", w_hours: 2, name: 'Wiring 2 Description', taskID: "1"},
        {id: "3", w_date: "2022-01-01", w_hours: 3, name: 'Wiring 3 Description', taskID: "1"}
      ],

      filter_projects:{id:'',name:'',active:''},
      filter_tasks:{id:'',name:'',active:''},
      filter_wirings:{taskName:'',name:'',date: ''},
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

  methods:{
    AddProject(newProject){
        this.current_projects.push(newProject);
    },

    DeleteProject(id)
    {
      for(let i = 0; i < this.current_projects.length; i++){
        if (this.current_projects.at(i).id === id){
          for(let j = this.current_tasks.length - 1; j >= 0; j--)
          {
            if (this.current_tasks.at(j).projectID === this.current_projects.at(i).id)
            {
              this.DeleteTask(j);
            }
          }
          this.current_projects.splice(i,1);
          break;
        }
      }
    },

    ChangeProject(id, newIDForProject,newNameForProject){
      for(let i = 0; i < this.current_projects.length; i++){
        if (this.current_projects.at(i).id === id){
          this.current_projects.at(i).name = newNameForProject;
          for(let j = 0; j < this.current_tasks.length; j++){
            if (this.current_tasks.at(j).projectID === this.current_projects.at(i).p_code){
              this.current_tasks.at(j).projectID = newIDForProject;
            }
          }
          this.current_projects.at(i).p_code = newIDForProject;
          break;
        }
      }
    },

    AddTask(newTask){
        this.current_tasks.push(newTask);
    },

    DeleteTask(id)
    {
      for(let i = 0; i < this.current_tasks.length; i++){
        if (this.current_tasks.at(i).id === id){
          for(let j = this.current_wirings.length - 1; j >= 0; j--)
          {
            if (this.current_wirings.at(j).taskID === this.current_tasks.at(i).id)
            {
              this.DeleteWiring(j);
            }
          }
          this.current_tasks.splice(i,1);
          break;
        }
      }
    },

    ChangeTask(id,newIDForTask,newNameForTask){
      for(let i = 0; i < this.current_tasks.length; i++){
        if (this.current_tasks.at(i).id === id){
          this.current_tasks.at(i).name = newNameForTask;
          this.current_tasks.at(i).projectID = newIDForTask;
          break;
        }
      }
    },

    DeleteWiring(id)
    {
      for(let i = 0; i < this.current_wirings.length; i++){
        if (this.current_wirings.at(i).id === id){
          this.current_wirings.splice(i,1);
          break;
        }
      }
    },

    ChangeWiring(id,newIDTask,inputNewWiringTaskHours,inputNewWiringName){
      for(let i = 0; i < this.current_wirings.length; i++){
        if (this.current_wirings.at(i).id === id){
          this.current_wirings.at(i).name = inputNewWiringName;
          this.current_wirings.at(i).w_hours = inputNewWiringTaskHours;
          this.current_wirings.at(i).taskID = newIDTask;
          break;
        }
      }
    },

    AddWiring(newWiring){
        this.current_wirings.push(newWiring);
    },

    ApplyFilterProjects(pID,pName,pActive){
      this.filter_projects.id = pID;
      this.filter_projects.name = pName;
      this.filter_projects.active = pActive;
    },

    ApplyFilterTasks(pID,pName,pActive){
      this.filter_tasks.id = pID;
      this.filter_tasks.name = pName;
      this.filter_tasks.active = pActive;
    },

    ApplyFilterWirings(pTaskName,pName){
      this.filter_wirings.taskName = pTaskName;
      this.filter_wirings.name = pName;
    },
  },

  computed:{
    FilterProjects() {
      if (this.filter_projects.active === 'all') return this.current_projects.filter(t => t.name.indexOf(this.filter_projects.name) > -1 && t.p_code.indexOf(this.filter_projects.id) > -1 );
      if (this.filter_projects.active === 'active') return this.current_projects.filter(t => t.active && t.name.indexOf(this.filter_projects.name) > -1 && t.p_code.indexOf(this.filter_projects.id) > -1);
      if (this.filter_projects.active === 'inactive') return this.current_projects.filter(t => !t.active && t.name.indexOf(this.filter_projects.name) > -1 && t.p_code.indexOf(this.filter_projects.id) > -1);
      return  this.current_projects;
    },

    FilterTasks() {
      if (this.filter_tasks.active === 'all') return this.current_tasks.filter(t => ((t.name.indexOf(this.filter_tasks.name) > -1) && t.projectID.indexOf(this.filter_tasks.id) > -1));
      if (this.filter_tasks.active === 'active') return this.current_tasks.filter(t => (t.active && (t.name.indexOf(this.filter_tasks.name) > -1) && t.projectID.indexOf(this.filter_tasks.id) > -1));
      if (this.filter_tasks.active === 'inactive') return this.current_tasks.filter(t => (!t.active && (t.name.indexOf(this.filter_tasks.name) > -1) && t.projectID.indexOf(this.filter_tasks.id) > -1));
      return  this.current_tasks;
    },

    FilterWirings() {
      let filtered_for_name = this.current_wirings.filter(t => ((t.name.indexOf(this.filter_wirings.name)) > -1));
      let filtered_task_for_name = this.current_tasks.filter(t => ((t.name.indexOf(this.filter_wirings.taskName)) > -1));
      let filtered_result = new Array;
      for (let i = 0; i < filtered_for_name.length; i++){
        for (let j = 0; j < filtered_task_for_name.length; j++){
          if(filtered_for_name.at(i).taskID === filtered_task_for_name.at(j).id){
            filtered_result.push(filtered_for_name.at(i));
            break;
          }
        }
      }
      return filtered_result;
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
    height:150px;
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
