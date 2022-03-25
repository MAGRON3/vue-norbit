<template>
    <div class="frame_div">        
        <div v-if="current_tasks.length">
            <strong>List</strong>
            <table width="500px">
                <tr>
                    <td width="5%">№</td>
                    <td width="10%">Active</td>
                    <td width="25%">Project Code</td>
                    <td width="45%">Name</td>
                    <td width="15%"></td>
                </tr>
                <TaskObj
                    v-for="(c_task,idx) in current_tasks"
                    v-bind:key="c_task.id"
                    v-bind:index="idx"
                    v-bind:c_task="c_task"
                    v-bind:current_projects="current_projects"
                    @delete-task="DeleteTask"
                    @change-task="ChangeTask"
                />
            </table>
        </div>
        <div v-else>
            <strong>No tasks</strong>
        </div>
    </div>
</template>

<script>
import TaskObj from '@/components/ListObjects/TaskObj.vue'
export default {
    props: {
        current_tasks: {
            type: Object,
            required: true
        },
        current_projects: {
            type: Object,
            required: true
        },
    },

    components: { TaskObj },

    data() {
        return{

        }
    },
    
    methods:{
        DeleteTask(taskID){
            this.$emit('delete-task',taskID)
        },

        ChangeTask(taskID,newTaskID,newNameForTask){
            this.$emit('change-task',taskID,newTaskID,newNameForTask)
        }
    },    
}
</script>

<style scoped>

</style>