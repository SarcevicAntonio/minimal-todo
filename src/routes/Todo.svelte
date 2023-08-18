<script>
	import { createEventDispatcher } from 'svelte'
	import { fade } from 'svelte/transition'

	const dispatch = createEventDispatcher()

	export let todo
	let editing = false
	let edit_title
</script>

<div transition:fade>
	<label class:done={todo.done}>
		<input type="checkbox" bind:checked={todo.done} />
		{#if !editing}
			{todo.title}
		{:else}
			<input type="text" bind:value={edit_title} />
		{/if}
	</label>
	{#if !editing}
		<button
			title="edit todo"
			on:click={() => {
				edit_title = todo.title
				editing = true
			}}
		>
			✏️
		</button>
		<button
			title="delete todo"
			on:click={() => {
				dispatch('delete', todo)
			}}
		>
			🗑️
		</button>
	{:else}
		<button
			title="submit edit"
			on:click={() => {
				if (!edit_title.trim().length) return
				todo.title = edit_title
				editing = false
			}}
		>
			☑️
		</button>
		<button
			title="cancel editing"
			on:click={() => {
				editing = false
			}}
		>
			❌
		</button>
	{/if}
</div>

<style>
	.done {
		text-decoration: line-through;
		opacity: 0.6;
	}
	div {
		display: flex;
		gap: 0.2rem;
		font-size: 1.2rem;
		border-radius: 0.25rem;
		background-color: white;
		padding: 0.25rem;
		border: 1px solid skyblue;
	}
	label {
		flex-grow: 1;
	}
	input {
		font: inherit;
		border: none;
		border-bottom: 1px solid lightgrey;
	}
	button {
		border: none;
		border-radius: 0.2rem;
	}
</style>
