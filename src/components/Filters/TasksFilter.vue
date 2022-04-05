<template>
    <div class="frame_div">
        <form @submit.prevent="ConfirmFilter">
        <div>
        <strong> Filter </strong>
        </div>
        <div>
            <input type="number" placeholder="Project Code" v-model="filterID">
        </div>
        <div>
            <input type="text" placeholder="Name" v-model="filterName">
        </div>
        <div>
        <select v-model="filterActive">
            <option value="all" selected="selected">All</option>
            <option value="active">Active</option>
            <option value="inactive">Inactive</option>
        </select>
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
            filterID: '',
            filterActive: 'all',
            filterName: '',
        }
    },

    methods:{
        ConfirmFilter(){
            this.$emit('apply-filter', this.filterID.toString(), this.filterName, this.filterActive);
        },

        ResetFilter(){
            this.filterID = '';
            this.filterActive = 'all';
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

select {
    margin: 2px;
    margin-top: 8px;
    width: 100%;
}
</style>