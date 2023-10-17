<script>
	export let dialog;
</script>

<dialog bind:this={dialog} on:close class="bg-transparent rounded-md border">
	<slot />
</dialog>

<style>
	::backdrop {
		background: black;
		opacity: 0.6;
	}
</style>
