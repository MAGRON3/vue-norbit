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
            <select v-model="filterDateType">
                <option value="all">All time</option>
                <option value="day">Day</option>
                <option value="month">Month</option>
            </select>
        </div>
        <div>
            <input type="date" v-if="filterDateType === 'day'" v-model="filterDate">
            <input type="month" v-if="filterDateType === 'month'" v-model="filterDate">
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
            filterDate: '',
            filterDateType: 'all',
        }
    }, 

    methods:{
        ConfirmFilter(){
            this.$emit('apply-filter', this.filterTaskName, this.filterName, this.filterDate,)
        },

        ResetFilter(){
            console.log(this.filterDate);
            this.filterTaskName = '';
            this.filterName = '';
            this.filterDate = '';
            this.filterDateType = 'all';
            this.ConfirmFilter();
        },
    },

    watch:{
        filterDateType(){
            this.filterDate = '';
        }
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

select {
    margin: 2px;
    margin-top: 8px;
    width: 100%;
}
</style>