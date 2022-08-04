<template>
	<h1>todos</h1>
	<form id="form" @submit.prevent="addTodo">
		<input
			type="text"
			class="input"
			id="input"
			placeholder="Enter your todo"
			autocomplete="off"
			v-model="newTodo"
		/>
		<p v-if="emptyValue" class="error">Black todo passed!!!</p>
		<ul class="todos" id="todos">
			<todo-item
				@delete-todo="removeTodo"
				v-for="todo in todosList"
				:key="todo.id"
				:todo="todo.todo"
			>
				{{ todo }}
			</todo-item>
		</ul>
	</form>
</template>

<script>
import TodoItem from './TodoItem.vue'
export default {
	components: {
		TodoItem,
	},
	data() {
		return {
			newTodo: '',
			todosList: [],
			emptyValue: false,
			apiError: null,
		}
	},
	mounted() {
		this.loadTodos()
	},
	methods: {
		async loadTodos() {
			const response = await fetch(
				'https://vue-demo-11a1f-default-rtdb.firebaseio.com/todos.json'
			)
			const resData = await response.json()
			const todos = []
			for (const id in resData) {
				todos.push({
					id: id,
					todo: resData[id].name,
				})
			}
			this.todosList = todos
		},
		addTodo() {
			const todoObject = {}
			if (this.newTodo === '') {
				this.emptyValue = true
				setTimeout(() => {
					this.emptyValue = false
				}, 3000)
			} else {
				todoObject.name = this.newTodo
				//this.todosList.push(todoObject)   //adding to an array
				// adding to an api
				fetch('https://vue-demo-11a1f-default-rtdb.firebaseio.com/todos.json', {
					method: 'POST',
					headers: {
						'Content-Type': 'application/json',
					},
					body: JSON.stringify(todoObject),
				})
			}
			this.newTodo = ''
		},
		removeTodo(todo) {
			this.todosList.pop(todo)
		},
	},
}
</script>

<style scoped>
h1 {
	color: rgb(179, 131, 226);
	font-size: 10rem;
	text-align: center;
	opacity: 0.4;
}

form {
	box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
	max-width: 100%;
	width: 400px;
}

.input {
	border: none;
	font-size: 2rem;
	color: #444;
	padding: 1rem 2rem;
	display: block;
	width: 100%;
}

.input::placeholder {
	color: #d5d5d5;
}
.input:focus {
	outline-color: rgb(179, 131, 226);
}

.todos {
	background-color: #fff;
	padding: 0;
	margin: 0;
	list-style-type: none;
}
.error {
	color: tomato;
}
</style>
