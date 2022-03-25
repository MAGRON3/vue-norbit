<template>
    <tr>
        <td>{{index + 1}}</td>
        <td>{{GetTaskName}}</td>
        <td>{{c_wiring.w_date}}</td>
        <td>{{c_wiring.w_hours}}</td>
        <td>{{c_wiring.name}}</td>
        <!-- <td>
            <div>
                <button class="btn_ed" @click="ChangeWiring">E</button>
                <button class="btn_rm" @click="DeleteWiring">&times;</button>
            </div>
        </td> -->
    </tr>
</template>

<script>
export default {
    props: {
        c_wiring:
        {
            type: Object,
            required: true
        },
        current_tasks:
        {
            type: Object,
            required: true
        },
        index: Number,
    },

    methods: {

    },

    computed:{
        GetTaskName(){
            for(let i = 0; i < this.current_tasks.length; i++){
                if (this.current_tasks.at(i).id === this.c_wiring.taskID) 
                    return this.current_tasks.at(i).name;
            }
            return '';
        }
    },

    data(){
        return{

        }
    },
}
</script>

<style scoped>
.btn_rm {
    background: red;
    color: #fff;
    border-radius: 50%;
    font-weight: bold;
    margin: 2px;
}

.btn_ed {
    background: green;
    color: #fff;
    border-radius: 50%;
    font-weight: bold;
    margin: 2px;
}

.input_edit{
    width: 95%;
    text-align: center;
}
</style>