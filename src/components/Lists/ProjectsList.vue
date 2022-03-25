<template>
    <div class="frame_div">        
        <div v-if="current_projects.length">
            <strong>List</strong>
            <table width="400px">
                <tr>
                    <td width="5%">№</td>
                    <td width="20%">Code</td>
                    <td width="60%">Name</td>
                    <td width="15%"></td>
                </tr>
                <ProjectObj
                    v-for="(c_project,idx) in current_projects"
                    v-bind:key="c_project.id"
                    v-bind:index="idx"
                    v-bind:c_project="c_project"
                    @delete-project="DeleteProject"
                    @change-project="ChangeProject"
                />
            </table>
        </div>
        <div v-else>
            <strong>Not projects</strong>
        </div>
    </div>
</template>

<script>
import ProjectObj from '@/components/ListObjects/ProjectObj.vue'
export default {
    props:['current_projects'],
    
    components: { ProjectObj },

    data() {
        return{

        }
    },
    
    methods:{
        DeleteProject(projectID){
            this.$emit('delete-project',projectID)
        },

        ChangeProject(projectID,newIDForProject,newNameForProject){
            this.$emit('change-project', projectID, newIDForProject, newNameForProject)
        },
    },
}
</script>

<style scoped>

</style>