<script lang="ts">
	export let id: string;
	export let title: string;
	export let done: boolean;
	export let desc: string;
	import { Trash2 } from 'lucide-svelte';
	import { createEventDispatcher } from 'svelte';
	const dispatch = createEventDispatcher();

	function handleDelete() {
		dispatch('delete', id);
	}
</script>

<li class="flex border-l-2 gap-2 p-3 rounded-lg bg-white justify-between">
	<div class="flex justify-center gap-2">
		<input type="checkbox" bind:checked={done} {id} />
		<label for={id} class={done ? 'line-through font-bold select-none ' : ''}>{title}</label>
	</div>
	<p class="block">
		{desc}
	</p>
	<button on:click={handleDelete}><Trash2 size={18} /></button>
</li>
