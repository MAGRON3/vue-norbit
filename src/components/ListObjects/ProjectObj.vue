<template>
    <tr>
        <td>{{index + 1}}</td>
        <td>
            <div v-if="!editMode">{{c_project.p_code}}</div>
            <input class="input_edit" v-else v-model="inputID">
        </td>
        <td>
            <span v-if="!editMode" v-bind:class="{is_done: !c_project.active}">{{c_project.name}}</span>
            <input class="input_edit" v-else v-model="inputName">
        </td>
        <td>
            <div>
                <button class="btn_ed" @click="ChangeProject" v-if="!editMode">E</button>
                <button class="btn_confirm" @click="ConfirmEdit" v-else>C</button>
                <button class="btn_rm" @click="DeleteProject" v-if="!editMode">&times;</button>
            </div>
        </td>
    </tr>
</template>

<script>
export default {
    props: {
        c_project:
        {
            type: Object,
            required: true
        },
        index: Number,
    },

    methods: {
        DeleteProject(){
            let result = confirm(`Delete "${this.c_project.name}" ?`);
            if (result) this.$emit('delete-project',this.c_project.id)
        },

        ChangeProject(){
            this.inputName = this.c_project.name;
            this.inputID = this.c_project.p_code;
            this.editMode = true;
        },

        ConfirmEdit(){
            this.$emit('change-project', this.c_project.id, this.inputID,this.inputName)
            this.editMode = false;
        }
    },

    data(){
        return{
            editMode: false,
            inputID: '',
            inputName: '',
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

.btn_confirm {
    background: blue;
    color: #fff;
    border-radius: 50%;
    font-weight: bold;
    margin: 2px;
}

.input_edit{
    width: 95%;
    text-align: center;
}

.is_done{
    text-decoration: line-through;
}
</style>