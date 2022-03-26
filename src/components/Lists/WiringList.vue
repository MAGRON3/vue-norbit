<template>
    <div class="frame_div">
        <div v-if="current_wirings.length">
            <strong>List</strong>
            <table width="100%">
                <tr>
                    <td width="3%">№</td>
                    <td width="20%">Task Name</td>
                    <td width="18%">Date</td>
                    <td width="5%">Hours</td>
                    <td width="35%">Description</td>
                    <td width="15%"></td>
                </tr>
                <WiringObj
                    v-for="(c_wiring,idx) in current_wirings"
                    v-bind:key="c_wiring.id"
                    v-bind:index="idx"
                    v-bind:c_wiring="c_wiring"
                    v-bind:current_tasks="current_tasks"
                    v-bind:current_wirings="current_wirings"
                    @delete-wiring="DeleteWiring"
                    @change-wiring="ChangeWiring"
                />
            </table>
        </div>
        <div v-else>
            <strong>No wirings</strong>
        </div>
    </div>
</template>

<script>
import WiringObj from '@/components/ListObjects/WiringObj.vue'
export default {
    props: {
        current_wirings:
        {
            type: Object,
            required: true
        },
        current_tasks:
        {
            type: Object,
            required: true
        },
    },

    components: { WiringObj },

    data() {
        return{
            
        }
    },
    
    methods:{
        DeleteWiring(id){
            this.$emit('delete-wiring',id);
        },

        ChangeWiring(id,newIDTask,inputNewWiringTaskHours,inputNewDate,inputNewWiringName){
            this.$emit('change-wiring',id,newIDTask,inputNewWiringTaskHours,inputNewDate,inputNewWiringName);
        }
    },
}
</script>

<style scoped>

</style>