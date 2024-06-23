<script>
	/**
	 * @type {{
	 *   title: string;
	 *   selected: import("@xyflow/svelte").NodeProps["selected"];
	 *   children: import("svelte").Snippet
	 * }}
	 */
	let { title, selected, children } = $props();
</script>

<div class="node" class:selected>
	<div class="title">{title}</div>
	{@render children()}
</div>

<style lang="scss">
	.node {
		background-color: #eee;
		border-radius: 0.5rem;
		overflow: auto;

		padding: 0.75rem;
		outline: 0px solid rgba(black, 0.2);
		transition: 50ms linear outline-width;

		display: grid;
		gap: 0.5rem;

		&.selected {
			outline-width: 3px;
		}
	}
</style>
