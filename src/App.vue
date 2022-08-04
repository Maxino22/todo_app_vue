<template>
	<todo-form></todo-form>
	<p>
		left click to toggle completed. <br />
		Right click to delete todo
	</p>
</template>

<script>
import TodoForm from './components/TodoForm.vue'
export default {
	components: {
		TodoForm,
	},
}
</script>
