<template>
    <div class="frame_div">
        <strong>Create new task</strong>
        <form @submit.prevent="AddTask">
            <div>
                <select v-model="inputNewTaskProjectCode">
                    <option value='' disabled>Select Project</option>
                    <option 
                        v-for="project in CurrentActiveProjects" 
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
    props: {
        current_projects:
        {
            type: Object,
            required: true
        },
    },
    data() {
        return{
            inputNewNameTask: '',
            inputNewTaskProjectCode: '',
        }
    },

    methods:{

        AddTask(){
            if (this.inputNewNameTask.length > 0 && 
            this.inputNewTaskProjectCode.toString().length > 0)
            {
                const newTask = {
                    name: this.inputNewNameTask,
                    project_code: this.inputNewTaskProjectCode,
                    active: true,
                };
                this.$emit('add-task',newTask);
                this.inputNewNameTask = '';
                this.inputNewTaskProjectCode = '';
            }
        },
    },

    computed:{
        CurrentActiveProjects(){
            return this.current_projects.filter(t => t.active);
        }
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
