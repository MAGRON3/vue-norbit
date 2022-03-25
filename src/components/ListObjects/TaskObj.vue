<template>
    <tr>
        <td>{{index + 1}}</td>
        <td>{{c_task.active}}</td>
        <td>
            <div v-if="!editMode">{{c_task.projectID}}</div>
            <div v-else>
                <select class="select_edit" v-model="inputID">
                    <option 
                        v-for="project in current_projects" 
                        v-bind:key="project.id"
                        v-bind:value="project.p_code"
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
            type: Object
        },
    },

    methods: {
        DeleteTask(){
            let result = confirm(`Delete "${this.c_task.name}" ?`);
            if (result) this.$emit('delete-task',this.c_task.id)
        },

        ChangeTask(){
            this.inputName = this.c_task.name;
            this.inputID = this.c_task.projectID;
            this.editMode = true;
        },

        ConfirmEdit(){
            this.$emit('change-task', this.c_task.id, this.inputID,this.inputName)
            this.editMode = false;
        }
    },

    data(){
        return{
            editMode: false,
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