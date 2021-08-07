<template>
    <section>
        <transition-group name="list" tag="ul">
            <li v-for="(task, index) in propsList" class="shadow" :key="task">
                <i class="checkBtn fas fa-check" aria-hidden="true"></i>
                {{ task.title }}
                <span class="removeBtn" type="button" @click="removeTask(task, index)">
                    <i class="far fa-trash-alt" aria-hidden="true"></i>
                </span>
            </li>
        </transition-group>
    </section>
</template>

<script>
export default {
    props: ['propsList', 'isAuthorized'],
    methods: {
        removeTask(task, index) {
            this.$emit('removeTask', task, index);
        }
    }
}
</script>

<style>
    ul {
        list-style-type: none;
        padding-left: 0px;
        margin-top: 0;
        text-align: left;
    }
    li {
        display: flex;
        min-height: 50px;
        height: 50px;
        line-height: 50px;
        margin: 0.5rem 0;
        padding: 0 0.9rem;
        background: white;
        border-radius: 5px;
    }
    .checkBtn {
        line-height: 45px;
        color: #62acde;
        margin-right: 5px;
    }
    .removeBtn {
        margin-left: auto;
        color: #de4343;
    }
    .list-enter-active, .list-leave-active {
        transition: all ls;
    }
    .list-enter, .list-leave-to {
        opacity: 0;
        transform: translateY(30px);
    }
</style>