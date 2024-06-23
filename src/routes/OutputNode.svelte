<script>
	import { Handle, Position, useHandleConnections, useSvelteFlow } from '@xyflow/svelte';
	import NodeContainer from './NodeContainer.svelte';

	/**
	 * @type {import("@xyflow/svelte").NodeProps}
	 */
	let { id, selected } = $props();

	const { getNodes } = useSvelteFlow();

	const connections = useHandleConnections({ nodeId: id, type: 'target' });
	const nodes = $derived(getNodes($connections.map((c) => c.source)));
</script>

<NodeContainer {selected} title="Output">
	<Handle type="target" position={Position.Left} />

	{#each nodes as node}
		<div>{node.data.value}</div>
	{:else}
		<div>No connected nodes</div>
	{/each}
</NodeContainer>
