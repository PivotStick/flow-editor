<script>
	import { Handle, Position, useHandleConnections, useSvelteFlow } from '@xyflow/svelte';
	import NodeContainer from './NodeContainer.svelte';

	/**
	 * @type {import("@xyflow/svelte").NodeProps}
	 */
	let { id, selected } = $props();

	const { updateNodeData, getNode } = useSvelteFlow();

	const connections = useHandleConnections({ nodeId: id, type: 'target' });

	/**
	 * @type {import('@xyflow/svelte').Node<any> | undefined}
	 */
	const node = $derived(getNode($connections[0]?.source));

	updateNodeData(
		id,
		{
			get value() {
				return node?.data.value?.toUpperCase() ?? '';
			}
		},
		{ replace: true }
	);
</script>

<NodeContainer {selected} title="ToUpperCase">
	<Handle type="target" position={Position.Left} isConnectable={$connections.length === 0} />
	<Handle type="source" position={Position.Right} />
</NodeContainer>
