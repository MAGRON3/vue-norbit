<template>
    <tr>
        <td>{{index + 1}}</td>
        <td>
            <div v-if="!editMode">{{c_task.active}}</div>
            <input type="checkbox" class="input_edit" v-else v-model="inputActive">
        </td>
        <td>
            <div v-if="!editMode">{{GetProjectPCode}}</div>
            <div v-else>
                <select class="select_edit" v-model="inputID">
                    <option 
                        v-for="project in GetActiveProjects" 
                        v-bind:key="project.id"
                        v-bind:value="project.id"
                    >
                        {{ project.name }}
                    </option>
                </select>
            </div>
        </td>
        <td>
            <span v-if="!editMode" v-bind:class="{is_done: !c_task.active}">{{c_task.name}}</span>
            <input class="input_edit" v-else v-model="inputName">
        </td>
        <td>
            <div>
                <button class="btn_ed" @click="ChangeTask" v-if="!editMode">E</button>
                <button class="btn_confirm" @click="ConfirmEdit" v-else>C</button>
                <button class="btn_rm" @click="DeleteTask" v-if="!editMode">&times;</button>
            </div>
        </td>
    </tr>
</template>

<script>
export default {
    props: {
        c_task: {
            type: Object,
            required: true
        },
        index: Number,
        current_projects: {
            type: Object,
            required: true
        },
    },

    methods: {
        DeleteTask(){
            let result = confirm(`Delete "${this.c_task.name}" ?`);
            if (result) this.$emit('delete-task',this.c_task.id);
        },

        ChangeTask(){
            this.inputActive = this.c_task.active;
            this.inputName = this.c_task.name;
            this.inputID = this.c_task.project_code;
            this.editMode = true;
        },

        ConfirmEdit(){
            this.$emit('change-task', this.c_task.id, this.inputActive, this.inputID,this.inputName);
            this.editMode = false;
        }
    },

    computed:{
        GetActiveProjects(){
            return this.current_projects.filter(t => t.active || t.id == this.c_task.project_code);
        },

        GetProjectPCode(){
            for(let i = 0; i <this.current_projects.length; i++){
                if (this.current_projects.at(i).id === this.c_task.project_code) return this.current_projects.at(i).p_code;
            }
            return '';
        },
    },

    data(){
        return{
            editMode: false,
            inputActive: '',
            inputID: '',
            inputName: '',
        }
    },
}
</script>

<style scoped>
.btn_rm {
    background: red;
    color: #fff;
    border-radius: 50%;
    font-weight: bold;
    margin: 2px;
}

.btn_ed {
    background: green;
    color: #fff;
    border-radius: 50%;
    font-weight: bold;
    margin: 2px;
}

.btn_confirm {
    background: blue;
    color: #fff;
    border-radius: 50%;
    font-weight: bold;
    margin: 2px;
}

.input_edit{
    width: 95%;
    text-align: center;
}

.select_edit{
    width: 95%;
    text-align: center;
}

.is_done{
    text-decoration: line-through;
}
</style>