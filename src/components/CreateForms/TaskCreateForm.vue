<template>
    <div class="frame_div">
        <strong>Create new task</strong>
        <form @submit.prevent="AddTask">
            <div>
                <select v-model="inputNewTaskProjectId">
                    <option value=''>Select Project</option>
                    <option 
                        v-for="project in current_projects" 
                        v-bind:key="project.id"
                        v-bind:value="project.id"
                    >
                        {{ project.name }}
                    </option>
                </select>
            </div>
            <input type="text" placeholder="Name" v-model="inputNewNameTask">
            <div>
                <button type="submit">Confirm</button>
            </div>
        </form>
    </div>
</template>

<script>
export default {
    props:['current_projects'],
    
    data() {
        return{
            inputNewNameTask: '',
            inputNewTaskProjectId: '',
        }
    },

    methods:{
        AddTask(){
            if (this.inputNewNameTask.trim() && toString(this.inputNewTaskProjectId).trim())
            {
                const newTask = {
                    id: Date.now(),
                    name: this.inputNewNameTask,
                    projectID: this.inputNewTaskProjectId,
                    active: true
                }
                this.$emit('add-task',newTask)
                this.inputNewNameTask = '';
                this.inputNewTaskProjectId = '';
            }
        },
    }
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
