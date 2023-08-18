<script>
	import { flip } from 'svelte/animate'
	import Todo from './Todo.svelte'

	let todos = [
		{ title: 'Learn Svelte', id: crypto.randomUUID(), done: false },
		{ title: 'Learn Imba', id: crypto.randomUUID(), done: false },
	]

	let new_title = ''

	$: todos.sort((a, b) => {
		return a.done - b.done
	})
</script>

<form
	on:submit|preventDefault={() => {
		if (!new_title.trim().length) return
		todos.push({ title: new_title, id: crypto.randomUUID(), done: false })
		todos = todos
		new_title = ''
	}}
>
	<label>
		New Todo:
		<input type="text" bind:value={new_title} />
	</label>
</form>

<ul>
	{#each todos as todo (todo.id)}
		<li animate:flip>
			<Todo
				bind:todo
				on:delete={({ detail: todo }) => {
					todos = todos.filter((t) => t.id !== todo.id)
				}}
			/>
		</li>
	{:else}
		<li>No todos...</li>
	{/each}
</ul>

<style>
	form {
		margin-bottom: 1rem;
	}
	label {
		display: flex;
		flex-direction: column;
	}
	input {
		border-radius: 0.25rem;
		border: 1px solid skyblue;
		font-size: 1.2rem;
		flex-grow: 1;
		padding: 0.25rem;
	}
	ul {
		list-style: none;
		margin: 0;
		padding: 0;
		display: flex;
		flex-direction: column;
		gap: 0.5rem;
		width: 100%;
	}
</style>
