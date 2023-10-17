<script lang="ts">
	import { PlusCircle } from 'lucide-svelte';
	import { createEventDispatcher } from 'svelte';
	import { v4 as uuid } from 'uuid';
	// create dispatcher
	const dispatch = createEventDispatcher<{
		add: { id: string; title: string; desc: string; done: boolean; dueDate: string };
	}>();

	let todoTitle = '';
	let todoDetails = '';
	let dueDate = '';

	function handleKeyDown(event: KeyboardEvent) {
		if (event.key === 'Enter') handleAdd();
	}

	function handleAdd() {
		// dispatch add addTodo event
		dispatch('add', {
			id: uuid(),
			title: todoTitle,
			desc: todoDetails,
			dueDate: dueDate,
			done: false
		});
	}
</script>

<div class="p-6 bg-white text-black w-[500px]">
	<form
		on:submit|preventDefault={() => {
			handleAdd();
		}}
	>
		<div class="grid gap-2">
			<input
				required
				class="flex h-10 w-full rounded-md border border-slate-300 bg-transparent px-3 py-1 text-sm transition-colors placeholder:text-muted-foreground focus-visible:outline-none focus-visible:ring-1 focus-visible:ring-ring disabled:cursor-not-allowed disabled:opacity-50"
				placeholder="Task Title"
				bind:value={todoTitle}
			/>

			<textarea
				required
				name="description"
				placeholder="Task Details"
				bind:value={todoDetails}
				class="flex min-h-[200px] w-full rounded-md border border-slate-300 bg-transparent px-3 py-1 text-sm shadow-sm transition-colors placeholder:text-muted-foreground focus-visible:outline-none focus-visible:ring-1 focus-visible:ring-ring disabled:cursor-not-allowed disabled:opacity-50"
			/>

			<input
				type="date"
				required
				class="flex h-9 w-full rounded-md border border-input border-slate-300 bg-transparent px-3 py-1 text-sm shadow-sm transition-colors placeholder:text-muted-foreground focus-visible:outline-none focus-visible:ring-1 focus-visible:ring-ring disabled:cursor-not-allowed disabled:opacity-50"
				bind:value={dueDate}
				placeholder="select a due date"
			/>
			<button type="submit" class="bg-blue-950 text-white rounded-md p-2 flex justify-center gap-4">
				<PlusCircle class="inline w-4" /> <span>Add Task</span>
			</button>
			<form />
		</div>
	</form>
</div>
