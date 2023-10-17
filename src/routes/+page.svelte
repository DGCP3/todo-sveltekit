<script lang="ts">
	import { PlusCircle } from 'lucide-svelte';
	import { onMount } from 'svelte';
	import '../app.css';
	import AddToDo from './components/AddToDo.svelte';
	import Date from './components/Date.svelte';
	import Dialog from './components/Dialog.svelte';
	import TodoItem from './components/TodoItem.svelte';

	type TypeTodo = {
		id: string;
		title: string;
		desc: string;
		done: boolean;
		dueDate: string;
	};

	let dialog: HTMLDialogElement;
	let toDos: TypeTodo[] = [];

	const addTodo = (e: CustomEvent<TypeTodo>) => {
		const newTodo = e.detail;
		if (newTodo.title.trim()) {
			toDos = [...toDos, { ...newTodo }];
			localStorage.setItem('toDoList', JSON.stringify(toDos));
			dialog.close();
		}
	};

	function deleteTodo(id: string) {
		toDos = toDos.filter((t) => t.id !== id);
		localStorage.setItem('toDoList', JSON.stringify(toDos));
	}

	onMount(() => {
		window.addEventListener('storage', (event) => {
			if (event.key === 'toDoList') {
				toDos = JSON.parse(event.newValue || '[]');
			}
		});
		if (localStorage.getItem('toDoList')) {
			toDos = JSON.parse(localStorage.getItem('toDoList') || '[]');
		}
		return () => {
			window.removeEventListener('storage', (event) => {
				if (event.key === 'toDoList') toDos = JSON.parse(event.newValue || '[]');
			});
		};
	});
</script>

<head>
	<link rel="preconnect" href="https://fonts.googleapis.com" />
	<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin="anonymous" />
	<link
		href="https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@300;400;500&display=swap"
		rel="stylesheet"
	/>
</head>

<main class="grid justify-center items-center h-screen bg-blue-950">
	<div class="bg-blue-200 min-h-[70vh] min-w-[400px] rounded-md p-4 place-content-start grid gap-4">
		<div class="flex justify-between">
			<h1 class="text-xl font-bold">Task List</h1>
			<button
				on:click={() => dialog.showModal()}
				class="font-bold bg-blue-950 px-3 py-3 rounded-lg"
			>
				<PlusCircle class="inline w-6 text-white" />
			</button>
		</div>
		<Date />
		<div class="space-y-6">
			<ul class="grid gap-2">
				{#each toDos as todo (todo.id)}
					<TodoItem {...todo} on:delete={() => deleteTodo(todo.id)} />
				{/each}
			</ul>
		</div>
	</div>
	<Dialog bind:dialog on:close={() => console.log('closed')}>
		<AddToDo on:add={addTodo} />
	</Dialog>
</main>

<style>
	/* @import url('https://fonts.googleapis.com/css2?family=Roboto+Mono:wght@100;200;300;400;500;700&display=swap'); */
	* {
		font-family: 'JetBrains Mono', monospace;
	}
</style>
