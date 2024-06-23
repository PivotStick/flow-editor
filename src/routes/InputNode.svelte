<script>
	import { Handle, Position, useSvelteFlow } from '@xyflow/svelte';
	import NodeContainer from './NodeContainer.svelte';

	/**
	 * @type {import("@xyflow/svelte").NodeProps<import("@xyflow/svelte").Node<any>>}
	 */
	let { id, data, selected } = $props();

	let title = $state(data.title ?? 'Input');
	let value = $state(data.value);

	const { updateNodeData } = useSvelteFlow();

	updateNodeData(
		id,
		{
			get title() {
				return title;
			},
			get value() {
				return value;
			}
		},
		{ replace: true }
	);
</script>

<NodeContainer {selected} bind:title>
	<input type="text" bind:value />
	<Handle type="source" position={Position.Right} />
</NodeContainer>
