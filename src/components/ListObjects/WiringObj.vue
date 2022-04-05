<template>
    <tr v-bind:class="{status_few: GetColorStatusInDay === 0, status_ok: GetColorStatusInDay === 1, status_many: GetColorStatusInDay === 2}">
        <td>{{index + 1}}</td>
        <td>
            
            <div v-if="editMode && GetTaskState">
                <select class="select_edit" v-model="inputNewIDTask">
                    <option 
                        v-for="task in GetAllActiveTask" 
                        v-bind:key="task.id"
                        v-bind:value="task.id"
                    >
                        {{ task.name }}
                    </option>
                </select>
            </div>
            <div v-else>{{GetTaskName}}</div>
        </td>
        <td>
            <div v-if="!editMode">{{c_wiring.w_date}}</div>
            <input class="input_edit" v-else type="date" v-model="inputNewDate">
        </td>
        <td>
            <div v-if="!editMode">{{c_wiring.w_hours}}</div>
            <div v-else>
                <select class="select_edit" v-model="inputNewHours">
                    <option v-if="GetLastHoursInDaySelect > 0" value='' disabled>Select Hours</option>
                    <option v-else value='' disabled>No more hours</option>
                    <option 
                        v-for="hours in GetLastHoursInDaySelect" 
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
        current_wirings:
        {
            type: Object,
            required: true
        },
        index: Number,
    },

    data(){
        return{
            editMode: false,
            inputNewIDTask: '',
            inputNewHours: '',
            inputNewName: '',
            inputNewDate: '',
            lastHoursEditMode: 24,
        }
    },

    methods: {
        DelteWiring(){
            let result = confirm(`Delete "${this.c_wiring.name}" ?`);
            if (result) this.$emit('delete-wiring',this.c_wiring.id);
        },

        ChangeWiring(){
            this.inputNewIDTask = this.c_wiring.task_code;
            this.inputNewHours = this.c_wiring.w_hours;
            this.inputNewName = this.c_wiring.name;
            this.inputNewDate = this.c_wiring.w_date;
            this.lastHoursEditMode = this.GetLastHoursInDay(this.c_wiring.w_date);
            this.editMode = true;
        },

        ConfirmEdit(){
            if (toString(this.inputNewIDTask).length > 0 &&
            this.inputNewHours <= this.GetLastHoursInDaySelect &&
            this.inputNewDate.length > 0 &&
            this.inputNewName.length)
                this.$emit('change-wiring',this.c_wiring.id,this.inputNewIDTask,this.inputNewHours,this.inputNewDate,this.inputNewName);
            this.editMode = false;
        },

        GetLastHoursInDay(currentDate){
            let lastHours = 24;
            for(let i = 0; i < this.current_wirings.length; i++){
                if (this.current_wirings.at(i).w_date === currentDate){
                    lastHours -= (this.current_wirings.at(i).w_hours);
                }
            }
            return lastHours;
        },
    },

    watch:{
        inputNewDate(){
            this.lastHoursEditMode = this.GetLastHoursInDay(this.inputNewDate);
            if(this.inputNewDate === this.c_wiring.w_date)
                this.inputNewHours = this.c_wiring.w_hours;
            if (this.inputNewHours > this.GetLastHoursInDaySelect) 
                this.inputNewHours = this.GetLastHoursInDaySelect;
        }
    },

    computed:{
        GetTaskName(){
            for(let i = 0; i < this.current_tasks.length; i++){
                if (this.current_tasks.at(i).id === this.c_wiring.task_code) 
                    return this.current_tasks.at(i).name;
            }
            return '';
        },

        GetTaskState(){
            for(let i = 0; i < this.current_tasks.length; i++){
                if (this.current_tasks.at(i).id === this.c_wiring.task_code) 
                    return this.current_tasks.at(i).active;
            }
            return false;
        },

        GetAllActiveTask(){
            return this.current_tasks.filter(t => t.active);
        },

        GetLastHoursInDaySelect(){
            if (this.c_wiring.w_date === this.inputNewDate)
                return this.lastHoursEditMode + this.c_wiring.w_hours;
                return this.lastHoursEditMode;
        },

        GetColorStatusInDay(){
            let lastHours = this.GetLastHoursInDay(this.c_wiring.w_date);
            if (lastHours > 16) return 0;
            if (lastHours == 16) return 1;
            return 2;
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

.status_ok{
    background: yellowgreen;
}

.status_few{
    background: yellow;
}

.status_many{
    background: lightcoral;
}
</style>