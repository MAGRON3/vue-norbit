<template>
    <div class="frame_div">
        <div v-if="current_wirings.length">
            <strong>List</strong>
            <table width="500px">
                <tr>
                    <td width="5%">№</td>
                    <td width="30%">Task Name</td>
                    <td width="20%">Date</td>
                    <td width="5%">Hours</td>
                    <td width="50%">Description</td>
                    <!-- <td width="15%"></td> -->
                </tr>
                <WiringObj
                    v-for="(c_wiring,idx) in current_wirings"
                    v-bind:key="c_wiring.id"
                    v-bind:index="idx"
                    v-bind:c_wiring="c_wiring"
                    v-bind:current_tasks="current_tasks"
                    @delete-wiring="DeleteWiring"
                    @change-wiring="ChangeWiring"
                />
            </table>
        </div>
        <div v-else>
            <strong>Not wirings</strong>
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

    },
}
</script>

<style scoped>

</style>