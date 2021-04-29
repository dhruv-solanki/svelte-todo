<script>
	import { slide } from "svelte/transition";
	import { elasticInOut } from "svelte/easing";

	let todos = [];
	let input = "";

	function addTodo() {
		if(input) {
			todos = [...todos, {
				text: input,
				id: Math.random().toString(36).substr(2, 9)
			}];
		}
		input = "";
	}

	function removeTodo(id) {
		const index = todos.findIndex(todo => todo.id === id);
		todos.splice(index, 1);
		todos = todos;
	}
</script>

<svelte:head>
	<link
    	rel="stylesheet"
    	type="text/css"
    	href="https://cdn.jsdelivr.net/npm/bulma@0.8.0/css/bulma.min.css"
  	/>
	<script src="https://use.fontawesome.com/releases/v5.3.1/js/all.js"></script>
</svelte:head>

<main class="container is-fluid">
	<div class="columns is-centered is-vcentered is-mobile">
		<div class="column is-narrow" style="width: 70%">
			<h1 class="has-text-centered title" style="margin-top: 50px;">Svelte TODO</h1>

			<form style="justify-content: center;" class="field has-addons" on:submit|preventDefault={addTodo}>
				<div class="control">
					<input type="text" bind:value={input} class="input" placeholder="TODO" />
				</div>
				<div class="control">
					<button class="button is-primary">
						<span class="icon is-small">
							<i class="fas fa-plus"></i>
						</span>
					</button>
				</div>
			</form>

			<ul class:list={todos.length > 0}>
				{#each todos as todo(todo.id)}
					<li class="list-item" transition:slide="{{duration: 300, easing: elasticInOut}}"> 
						<div class="is-flex" style="align-items: center;">
							<span class="is-pulled-left"> {todo.text} </span>
							<div style="flex: 1;"></div>
							<button class="button is-text is-pulled-right is-small" on:click={() => removeTodo(todo.id)}>
								<span class="icon">
									<i class="fas fa-check"></i>
								</span>
							</button>
						</div>
					</li>
				{:else}
					<li class="has-text-centered" transition:slide="{{delay: 600, duration: 300, easing: elasticInOut}}">
						Nothing Here!
					</li>
				{/each}
			</ul>
		</div>
	</div>
</main>
