<template>
    <div v-if="isAuthorized" class="inputBox shadow">
        <input type="text" v-model="newTaskItem" placeholder="Type what you have to do" v-on:keyup.enter="addTask">
        <span class="addContainer" v-on:click="addTask">
            <i class="addBtn fas fa-plus" aria-hidden="true"></i>
        </span>
    </div>
</template>

<script>
export default {
    props: ['isAuthorized'],
    data() {
        return {
            newTaskItem: ''
        }
    },
    methods: {
        addTask() {
            if (this.newTaskItem !== "") {
                var value = this.newTaskItem.trim();
                this.$emit('addTask', value);
                this.clearInput();
            }
        },
        clearInput() {
            this.newTaskItem = '';
        }
    }
}
</script>


<style scoped>
    input:focus {
        outline: none;
    }
    .inputBox {
        background: white;
        height: 50px;
        line-height: 50px;
        border-radius: 5px;
    }
    .inputBox input {
        border-style: none;
        font-size: 0.9rem;
    }
    .addContainer {
        float: right;
        background: linear-gradient(to right, #0d6efd, #0d6efd);
        display: block;
        width: 3rem;
        border-radius: 0 5px 5px 0;
    }
    .addBtn {
        color: white;
        vertical-align: middle;
    }
</style>