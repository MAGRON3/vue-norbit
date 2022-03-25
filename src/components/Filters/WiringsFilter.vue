<template>
    <div class="frame_div">
        <form @submit.prevent="ConfirmFilter">
        <strong> Filter </strong>
        <div>
            <input type="text" placeholder="Task Name" v-model="filterTaskName">
        </div>
        <div>
            <input type="text" placeholder="Name" v-model="filterName">
        </div>
        <div>
            <input type="date">
        </div>
        <div>
            <button class="btn" type="submit">Apply</button>
            <button class="btn" @click="ResetFilter">Reset</button>
        </div>
        </form>
    </div>
</template>

<script>
export default {
    data() {
        return{
            filterTaskName: '',
            filterName: '',
        }
    }, 

    methods:{
        ConfirmFilter(){
            this.$emit('apply-filter', this.filterTaskName, this.filterName)
        },

        ResetFilter(){
            this.filterTaskName = '';
            this.filterName = '';
            this.ConfirmFilter();
        },
    }
}
</script>

<style scoped>
.btn{
    margin: 2px;
    margin-top: 8px;
    width: 90px;
}

input{
    margin: 2px;
    margin-top: 8px;
    width: 100%;
}
</style>