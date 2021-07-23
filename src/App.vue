<template>
    <div id="app">
        <Header v-on:getAccessTokenHandler="getAccessTokenHandler"></Header>
        <Input v-on:addTask="addTask"></Input>
        <List v-bind:propsdata="taskList" @removeTask="removeTask"></List>
        <Footer v-on:removeAll="clearAll"></Footer>
    </div>
</template>

<script>
import Header from './components/Header.vue'
import Input from './components/Input.vue'
import List from './components/List.vue'
import Footer from './components/Footer.vue'
export default {
	data() {
		return {
			taskList: []
		}
	},
	methods: {
		addTask(task) {
			alert("called addTask " + task);
		},
		clearAll() {
			alert("called clearAll");
		},
		removeTask(task) {
			alert("called removeTask");
		},
		getAccessTokenHandler(userInfo) {
			if (userInfo !== null) {
				var string = `email = ${userInfo.email}\nname = ${userInfo.name}\naccessToken=${userInfo.accessToken}`
				alert(string)
			}
		},
	},
	components: {
		'Header': Header,
		'Input': Input,
		'List': List,
		'Footer': Footer
	},
	created() {
		alert("call created");
	}
}
</script>

<style>
    body {
        text-align: center;
        background-color: #F6F6F8;
    }
    input {
        border-style: groove;
        width: 200px;
    }
    button {
        border-style: groove;
    }
    .shadow {
        box-shadow: 5px 10px 10px rgba(0, 0, 0, 0.03)
    }
</style>
