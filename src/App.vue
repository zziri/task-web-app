<template>
    <div id="app">
        <Header v-on:googleLogin="googleLogin" v-bind:isAuthorized="isAuthorized"></Header>
        <Input v-on:addTask="addTask" v-bind:isAuthorized="isAuthorized"></Input>
        <List v-bind:propsList="taskList" @removeTask="removeTask" v-bind:isAuthorized="isAuthorized"></List>
        <Footer v-on:removeAll="clearAll" v-bind:isAuthorized="isAuthorized"></Footer>
    </div>
</template>

<script>
import Header from './components/Header.vue'
import Input from './components/Input.vue'
import List from './components/List.vue'
import Footer from './components/Footer.vue'
import axios from 'axios'

export default {
	data() {
		return {
			taskList: [],
			accessToken: "",
			jwt: "",
			isAuthorized: false,
		}
	},

	methods: {
		async addTask(taskTitle) {
			var response = await this.createTaskRequest(taskTitle)
			var task = response.data.data
			this.taskList.push(task)
		},

		async clearAll() {
			for (var i=0; i<this.taskList.length; i++) {
				var response = await this.deleteTaskRequest(this.taskList[i].id)
				console.log(response)
			}
			this.taskList = []
		},

		async removeTask(task, index) {
			var response = await this.deleteTaskRequest(task.id)
			console.log(response)
			this.taskList.splice(index, 1);
			return true
		},

		async googleLogin(userInfo) {
			if (userInfo !== null) {
				this.accessToken = userInfo.accessToken
				var response = await this.signInRequest()
				console.log(`googleLogin request\n${response}`)
				this.taskList = await this.getTasks()
				console.log(`taskList = ${JSON.stringify(this.taskList)}`)
				this.isAuthorized = true
			}
		},

		signInRequest() {
			return axios({
				url: 'https://task.zziri.me/v2/auth/google?accessToken=' + this.accessToken,
				headers: {
					'Content-Type': 'application/json'
				},
				method: 'POST'
			})
			.then((response) => {
				this.jwt = response.data.data
				console.log("jwt = " + this.jwt)
				return response
			})
		},

		createTaskRequest(taskTitle) {
			return axios({
				url: 'https://task.zziri.me/v2/tasks',
				headers: {
					'Content-Type': 'application/json',
					'Task-Authentication': this.jwt
				},
				method: 'POST',
				data: {
					title: taskTitle
				}
			})
		},

		async getTasks() {
			var response = await this.getTasksReqeust()
			return response.data.data
		},

		getTasksReqeust() {
			return axios({
				url: 'https://task.zziri.me/v2/tasks',
				headers: {
					'Content-Type': 'application/json',
					'Task-Authentication': this.jwt
				},
				method: 'GET'
			})
		},

		deleteTaskRequest(id) {
			return axios({
				url: 'https://task.zziri.me/v2/tasks/' + id,
				headers: {
					'Content-Type': 'application/json',
					'Task-Authentication': this.jwt
				},
				method: 'DELETE'
			})
		}
	},

	components: {
		'Header': Header,
		'Input': Input,
		'List': List,
		'Footer': Footer
	},
	
	created() {
		console.log("call created()")
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
	#app {
		height: 100%;
		max-width: 70%;
		min-width: 70%;
		display: inline-block;
	}
</style>
