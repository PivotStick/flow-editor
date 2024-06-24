<script>
	import { Background, SvelteFlow, useSvelteFlow } from '@xyflow/svelte';
	import ScreenNode from './ScreenNode.svelte';
	import { writable } from 'svelte/store';
	import { onMount, tick } from 'svelte';

	const nodes = writable([
		{
			id: '1',
			type: 'ScreenNode',
			position: { x: 0, y: 0 },
			data: {
				state: 'unclaimed',
				persona: 'anonymous'
			}
		},
		{
			id: '2',
			type: 'ScreenNode',
			position: { x: 600, y: 0 },
			data: {
				state: 'claimed',
				persona: 'anonymous'
			}
		},
		{
			id: '3',
			type: 'ScreenNode',
			position: { x: 1200, y: 0 },
			data: {
				state: 'claimed',
				persona: 'owner'
			}
		}
	]);

	const edges = writable([]);

	/** @type {any} */
	const nodeTypes = {
		ScreenNode
	};

	const { fitView } = useSvelteFlow();

	onMount(() => {
		setTimeout(fitView, 0);
	});
</script>

<SvelteFlow {nodes} {edges} {nodeTypes}>
	<Background bgColor="#fafafa" />
</SvelteFlow>
