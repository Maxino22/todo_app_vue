<template>
	<li
		@click="isCompleted"
		@contextmenu.prevent="deleteTodo"
		:class="completedClass"
	>
		{{ todo }}
	</li>
</template>

<script>
export default {
	props: ['todo'],
	data() {
		return {
			completed: false,
		}
	},
	computed: {
		completedClass() {
			return { completed: this.completed }
		},
	},
	methods: {
		isCompleted() {
			this.completed = !this.completed
		},
		deleteTodo() {
			this.$emit('delete-todo', this.todo)
		},
	},
}
</script>

<style scoped>
li {
	border-top: 1px solid #e5e5e5;
	cursor: pointer;
	font-size: 1.5rem;
	padding: 1rem 2rem;
}

.completed {
	color: #b6b6b6;
	text-decoration: line-through;
}
</style>
