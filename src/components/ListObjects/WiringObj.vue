<template>
    <tr>
        <td>{{index + 1}}</td>
        <td>
            <div v-if="!editMode">{{GetTaskName}}</div>
            <div v-else>
                <select class="select_edit" v-model="inputNewIDTask">
                    <option 
                        v-for="task in current_tasks" 
                        v-bind:key="task.id"
                        v-bind:value="task.id"
                    >
                        {{ task.name }}
                    </option>
                </select>
            </div>
        </td>
        <td>{{c_wiring.w_date}}</td>
        <td>
            <div v-if="!editMode">{{c_wiring.w_hours}}</div>
            <div v-else>
                <select class="select_edit" v-model="inputNewHours">
                    <option 
                        v-for="hours in 24" 
                        v-bind:key="hours"
                        v-bind:value="hours"
                    >
                        {{ hours }}
                    </option>
                </select>
            </div>
        </td>
        <td>
            <div v-if="!editMode">{{c_wiring.name}}</div>
            <input class="input_edit" v-else v-model="inputNewName" type="text">
        </td>
        <td>
            <div>
                <button class="btn_ed" v-if="!editMode" @click="ChangeWiring">E</button>
                <button class="btn_confirm" v-else @click="ConfirmEdit">C</button>
                <button class="btn_rm" v-if="!editMode" @click="DelteWiring">&times;</button>
            </div>
        </td>
    </tr>
</template>

<script>
export default {
    props: {
        c_wiring:
        {
            type: Object,
            required: true
        },
        current_tasks:
        {
            type: Object,
            required: true
        },
        index: Number,
    },

    methods: {
        DelteWiring(){
            let result = confirm(`Delete "${this.c_wiring.name}" ?`);
            if (result) this.$emit('delete-wiring',this.c_wiring.id)
        },

        ChangeWiring(){
            this.inputNewIDTask = this.c_wiring.taskID;
            this.inputNewHours = this.c_wiring.w_hours;
            this.inputNewName = this.c_wiring.name;
            this.editMode = true;
        },

        ConfirmEdit(){
            this.$emit('change-wiring',this.c_wiring.id,this.inputNewIDTask,this.inputNewHours,this.inputNewName);
            this.editMode = false;
        }
    },

    computed:{
        GetTaskName(){
            for(let i = 0; i < this.current_tasks.length; i++){
                if (this.current_tasks.at(i).id === this.c_wiring.taskID) 
                    return this.current_tasks.at(i).name;
            }
            return '';
        }
    },

    data(){
        return{
            editMode: false,
            inputNewIDTask: '',
            inputNewHours: '',
            inputNewName: '',
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

.input_edit{
    width: 95%;
    text-align: center;
}
</style>