<script>
	/**
	 * @type {{
	 *   title: string;
	 *   selected: import("@xyflow/svelte").NodeProps["selected"];
	 *   children: import("svelte").Snippet
	 * }}
	 */
	let { title = $bindable(), selected, children } = $props();
</script>

<div class="node" class:selected>
	<input disabled type="text" bind:value={title} placeholder="Node's title" />
	{@render children()}
</div>

<style lang="scss">
	.node {
		background-color: rgba(var(--color-300));
		border-radius: 0.5rem;
		overflow: auto;

		padding: 0.75rem;
		outline: 0px solid rgba(var(--color-500) / 20%);
		transition: 75ms linear outline-width;

		display: grid;
		gap: 0.5rem;

		&.selected {
			outline-width: 3px;
		}

		input {
			background: none;
			border: none;
			font-size: inherit;

			&:disabled {
				pointer-events: none;
			}
		}
	}
</style>
