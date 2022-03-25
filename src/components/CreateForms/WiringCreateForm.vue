<template>
    <div class="frame_div">
        <strong>Create new wiring</strong>
        <form @submit.prevent="AddWiring">
            <div>
            <select v-model="inputNewWiringTaskId">
                <option value='' disabled>Select Task</option>
                <option 
                    v-for="task in CurrentActiveTask" 
                    v-bind:key="task.id"
                    v-bind:value="task.id"
                >
                    {{ task.name }}
                </option>
            </select>
            <input type="date" v-model="inputNewWiringDate">
            <select v-model="inputNewWiringHours">
                <option v-if="GetLastHoursInCurrentDay > 0" value='' disabled>Select Hours</option>
                <option v-else value='' disabled>No more hours</option>
                <option 
                    v-for="(hour,i) in GetLastHoursInCurrentDay" 
                    v-bind:key="hour"
                    v-bind:value="i+1"
                >
                    {{ hour }}
                </option>
            </select>
            </div>
            <input type="text" placeholder="Name" v-model="inputNewWiringName">
            <div>
                <button type="submit">Confirm</button>
            </div>
        </form>
    </div>
</template>

<script>
export default {
    props: {
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
    },
    data() {
        return{
            inputNewWiringName: '',
            inputNewWiringTaskId: '',
            inputNewWiringDate: '',
            inputNewWiringHours: '',
        }
    },

    methods:{
        GetCurrentDateSting()
        {
            let current_date = new Date();
            return current_date.getFullYear() + '-' + ((current_date.getMonth() + 1) < 10 ? '0' + (current_date.getMonth() + 1) : (current_date.getMonth() + 1)) + '-' + (current_date.getDate() < 10 ? '0' + current_date.getDate() : current_date.getDate());
        },

        AddWiring(){
            
            if (this.inputNewWiringName.length > 0 && 
            this.inputNewWiringTaskId.length > 0 &&
            this.inputNewWiringDate.length > 0 &&
            this.inputNewWiringHours > 0)
            {         
                const newWiring = {
                    id: Date.now(),
                    w_date: this.inputNewWiringDate,
                    w_hours: this.inputNewWiringHours,
                    name: this.inputNewWiringName,
                    taskID: this.inputNewWiringTaskId,
                    active: true
                }
                this.$emit('add-wiring',newWiring)
                this.inputNewWiringName = '';
                this.inputNewWiringTaskId = '';
                this.inputNewWiringHours = '';
                this.inputNewWiringDate = '';
            }
        },
    },

    computed:{
        CurrentActiveTask(){
            return this.current_tasks.filter(t => t.active);
        },

        GetLastHoursInCurrentDay(){
            let currentDate = this.inputNewWiringDate;
            let lastHours = 24;
            if(this.inputNewWiringDate.trim())
            {
                for(let i = 0; i < this.current_wirings.length; i++){
                    if (this.current_wirings.at(i).w_date === currentDate){
                        lastHours -= (this.current_wirings.at(i).w_hours);
                    }
                }
            } else lastHours = 0;
            return lastHours;
        },
    },
}
</script>

<style scoped>
select{
    margin: 2px;
    margin-top: 8px;
    width: 100%;
}

input{
    margin: 2px;
    margin-top: 8px;
    width: 100%;
}

button{
    margin: 2px;
    margin-top: 8px;
    width: 90px;
}
</style>
