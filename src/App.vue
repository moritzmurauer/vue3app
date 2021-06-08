<template>
	<h1>ToDo App</h1>
	<form @submit.prevent="addTodo">
		<label>Neues ToDo </label>
		<input
			v-model="newTodo"
			name="newTodo"
			autocomplete="off"
		>
		<button>ToDo Hinzufügen</button>
	</form>
	<h2>ToDo Liste</h2>
	<ul>
		<li	
			v-for="(todo, index) in todos"
			:key="index"
		>
			<span
				:class="{ done: todo.done }"
				@click="doneTodo(todo)"
			>{{ todo.content }} 
			</span>
			<span 
				v-if="todo.done" 
				@click="doneTodo(todo)" 
				style='font-size:24px;'
				>&#9989;</span>
			<button @click="removeTodo(index)">Entfernen</button>
		</li>
	</ul>
	<h4 v-if="todos.length === 0">Keine Einträge.</h4>
</template>

<script>
	import { ref } from 'vue';
	export default {
		name: 'App',
		setup () {
			const newTodo = ref('');
			const defaultData = [{
				done: false,
				content: 'Schreib ein Todo'
			}]
			const todosData = JSON.parse(localStorage.getItem('todos')) || defaultData;
			const todos = ref(todosData);

			const saveData = () => {
				const storageData = JSON.stringify(todos.value);
				localStorage.setItem('todos', storageData);
			}

			const addTodo = () => {
				if (newTodo.value) {
					todos.value.push({
						done: false,
						content: newTodo.value
					});
					newTodo.value = '';
				}
				saveData();
			}

			const doneTodo = (todo) => {
				todo.done = !todo.done
				saveData();
			}

			const removeTodo =  (index) => {
				todos.value.splice(index, 1);
				saveData();
			}

			

			return {
				todos,
				newTodo,
				addTodo,
				doneTodo,
				removeTodo,
				saveData
			}
		}
	}
</script>

<style lang="scss">
$border: 2px solid
	rgba(
		$color: white,
		$alpha: 0.35,
	);
$size1: 6px;
$size2: 12px;
$size3: 18px;
$size4: 24px;
$size5: 48px;
$backgroundColor: #60a16b;
$textColor: white;
$primaryColor: #226237;
$secondTextColor: #ffffff;
body {
	margin: 0;
	padding: 0;
	font-family: Avenir, Helvetica, Arial, sans-serif;
	-webkit-font-smoothing: antialiased;
	-moz-osx-font-smoothing: grayscale;
	background-color: $backgroundColor;
	color: $textColor;
	#app {
		max-width: 600px;
		margin-left: auto;
		margin-right: auto;
		padding: 20px;
		h1 {
			font-weight: bold;
			font-size: 28px;
			text-align: center;
		}
		form {
			display: flex;
			flex-direction: column;
			width: 100%;
			label {
				font-size: 14px;
				font-weight: bold;
			}
			input,
			button {
				height: $size5;
				box-shadow: none;
				outline: none;
				padding-left: $size2;
				padding-right: $size2;
				border-radius: $size1;
				font-size: 18px;
				margin-top: $size1;
				margin-bottom: $size2;
			}
			input {
				background-color: transparent;
				border: $border;
				color: inherit;
			}
		}
		button {
			cursor: pointer;
			background-color: $primaryColor;
			border: 1px solid $primaryColor;
			color: $secondTextColor;
			font-weight: bold;
			outline: none;
			border-radius: $size1;
		}
		h2 {
			font-size: 22px;
			border-bottom: $border;
			padding-bottom: $size1;
		}
		ul {
			padding: 10px;
			li {
				display: flex;
				justify-content: space-between;
				align-items: center;
				border: $border;
				padding: $size2 $size4;
				border-radius: $size1;
				margin-bottom: $size2;
				span {
					cursor: pointer;
				}
				.done {
					text-decoration: line-through;
					background: green;
				}
				button {
					font-size: $size2;
					padding: $size1;
				}
			}
		}
		h4 {
			text-align: center;
			opacity: 0.5;
			margin: 0;
		}
	}
}
</style>
