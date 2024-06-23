<script>
	import { Background, SvelteFlow, useSvelteFlow } from '@xyflow/svelte';
	import { writable } from 'svelte/store';
	import CodeEditorNode from './CodeEditorNode.svelte';
	import InputNode from './InputNode.svelte';
	import OutputNode from './OutputNode.svelte';
	import ToUpperCase from './ToUpperCase.svelte';

	const key = '__DATAS__';
	const initial = JSON.parse(localStorage.getItem(key) ?? JSON.stringify({ nodes: [], edges: [] }));

	/**
	 * @type {import('svelte/store').Writable<import("@xyflow/svelte").Node[]>}
	 */
	const nodes = writable(initial.nodes);

	/**
	 * @type {import('svelte/store').Writable<import("@xyflow/svelte").Edge[]>}
	 */
	const edges = writable(initial.edges);

	const { screenToFlowPosition } = useSvelteFlow();

	$effect(() => {
		const json = JSON.stringify({ nodes: $nodes, edges: $edges });
		localStorage.setItem(key, json);
	});

	/** @type {any} */
	const nodeTypes = {
		CodeEditorNode,
		InputNode,
		OutputNode,
		ToUpperCase
	};

	/**
	 * @param {DragEvent} e
	 */
	function onDrop(e) {
		e.preventDefault();

		if (!e.dataTransfer) return;

		const type = e.dataTransfer.getData('application/svelteflow');
		const position = screenToFlowPosition({
			y: e.clientY,
			x: e.clientX
		});

		nodes.update((n) => {
			n.push({
				id: crypto.randomUUID(),
				type,
				position,
				data: {},
				origin: [0.5, 0.5]
			});

			return n;
		});
	}
</script>

<div class="container">
	<div class="nodes">
		{#each Object.keys(nodeTypes) as name}
			<button
				draggable="true"
				ondragstart={(e) => {
					if (e.dataTransfer) {
						e.dataTransfer.setData('application/svelteflow', name);
					}
				}}
			>
				{name}
			</button>
		{/each}
	</div>
	<main>
		<SvelteFlow
			{nodeTypes}
			{nodes}
			{edges}
			on:dragover={(e) => e.preventDefault()}
			on:drop={onDrop}
		>
			<Background />
		</SvelteFlow>
	</main>
</div>

<style lang="scss">
	.container {
		height: 100vh;
		display: grid;
		grid-template-columns: auto 1fr;

		.nodes {
			border-right: 1px solid #eee;
			padding: 2rem;

			display: flex;
			flex-direction: column;
			gap: 1rem;
		}
	}
</style>
