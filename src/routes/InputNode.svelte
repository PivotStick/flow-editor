<script>
	import { Handle, Position, useSvelteFlow } from '@xyflow/svelte';
	import NodeContainer from './NodeContainer.svelte';

	/**
	 * @type {import("@xyflow/svelte").NodeProps<import("@xyflow/svelte").Node<{ value: string }>>}
	 */
	let { id, data, selected } = $props();

	let value = $state(data.value);

	const { updateNodeData } = useSvelteFlow();

	updateNodeData(
		id,
		{
			get value() {
				return value;
			}
		},
		{ replace: true }
	);
</script>

<NodeContainer {selected} title="Input">
	<input type="text" bind:value />
	<Handle type="source" position={Position.Right} />
</NodeContainer>
