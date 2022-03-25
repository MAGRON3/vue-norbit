<template>
    <div class="frame_div">
        <strong>Create new project</strong>
        <form @submit.prevent="AddProject">
            <input type="text" placeholder="Name" v-model="inputNewProjectName">
            <input type="text" placeholder="Code" v-model="inputNewProjectCode">
            <div>
                <button type="submit">Confirm</button>
            </div>
        </form>
    </div>
</template>

<script>
export default {
    data() {
        return{
            inputNewProjectName: '',
            inputNewProjectCode: '',
        }
    },
    
    methods:{
        AddProject(){
            if (this.inputNewProjectName.length > 0 &&
                this.inputNewProjectCode.length > 0) {
                const newProject = {
                    id: Date.now(),
                    p_code: this.inputNewProjectCode,
                    name: this.inputNewProjectName,
                    active: true,
                };
                this.$emit('add-project',newProject);
                this.inputNewProjectName = '';
                this.inputNewProjectCode = '';
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