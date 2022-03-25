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
            <select v-model="inputNewWiringTaskHours">
                <option 
                    v-for="(hour,i) in 24" 
                    v-bind:key="hour"
                    v-bind:value="i"
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
    props:['current_tasks'],

    data() {
        return{
            inputNewWiringName: '',
            inputNewWiringTaskId: '',
            inputNewWiringTaskHours: '0',
        }
    },

    methods:{
        GetCurrentDateSting()
        {
            let current_date = new Date();
            return current_date.getFullYear() + '-' + ((current_date.getMonth() + 1) < 10 ? '0' + (current_date.getMonth() + 1) : (current_date.getMonth() + 1)) + '-' + (current_date.getDate() < 10 ? '0' + current_date.getDate() : current_date.getDate());
        },

        AddWiring(){
            if (this.inputNewWiringName.trim() && toString(this.inputNewWiringTaskId).trim())
            {               
                const newWiring = {
                    id: Date.now(),
                    w_date: this.GetCurrentDateSting(),
                    w_hours: this.inputNewWiringTaskHours,
                    name: this.inputNewWiringName,
                    taskID: this.inputNewWiringTaskId,
                    active: true
                }
                this.$emit('add-wiring',newWiring)
                this.inputNewWiringName = '';
                this.inputNewWiringTaskId = '';
                this.inputNewWiringTaskHours = '0'
            }
        },
    },

    computed:{
        CurrentActiveTask(){
            return this.current_tasks.filter(t => t.active);
        }
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
