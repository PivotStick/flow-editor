<script>
	import { indentWithTab } from '@codemirror/commands';
	import { EditorState } from '@codemirror/state';
	import { keymap } from '@codemirror/view';
	import { javascript } from '@codemirror/lang-javascript';
	import { Handle, Position, useHandleConnections, useSvelteFlow } from '@xyflow/svelte';
	import { EditorView, basicSetup } from 'codemirror';
	import { onMount } from 'svelte';
	import NodeContainer from './NodeContainer.svelte';
	import { slide } from 'svelte/transition';
	import { expoOut } from 'svelte/easing';

	/**
	 * @type {import('@xyflow/svelte').NodeProps & { data: any }}
	 */
	let { id, data, selected } = $props();

	/**
	 * @type {HTMLElement=}
	 */
	let container = $state();

	/**
	 * @type {EditorView=}
	 */
	let view = $state();

	const { updateNodeData, getNode } = useSvelteFlow();

	let title = $state(data.title ?? 'Code');
	let error = $state();
	let value = $state();
	let code = $state(
		data.code ??
			`export default function(input) {
    return input; // output;
}`
	);

	const connections = useHandleConnections({ nodeId: id, type: 'target' });

	/**
	 * @type {import('@xyflow/svelte').Node<any> | undefined}
	 */
	const node = $derived(getNode($connections[0]?.source));

	updateNodeData(
		id,
		{
			get title() {
				return title;
			},
			get value() {
				return value;
			},
			get code() {
				return code;
			}
		},
		{ replace: true }
	);

	$effect(() => {
		const input = node?.data.value;

		import('data:text/javascript;base64,' + btoa(data.code ?? ''))
			.then((module) => {
				error = undefined;
				if (typeof module.default === 'function') {
					value = module.default(input);
				}
			})
			.catch((e) => (error = e));
	});

	onMount(() => {
		const v = (view = new EditorView({
			parent: container,
			state: EditorState.create({
				doc: code,
				extensions: [basicSetup, keymap.of([indentWithTab]), javascript()]
			}),
			dispatchTransactions(trs) {
				v.update(trs);
				if (trs.some((tr) => tr.docChanged)) {
					code = v.state.doc.toString();
				}
			}
		}));

		return () => {
			view?.destroy();
		};
	});
</script>

<NodeContainer {selected} bind:title>
	<div class="editor" class:error bind:this={container}></div>

	{#if error}
		<pre in:slide|local={{ easing: expoOut }} style="color: red; margin-top: .5rem;">{error}</pre>
	{/if}

	<Handle type="target" position={Position.Left} />
	<Handle type="source" position={Position.Right} />
</NodeContainer>

<style lang="scss">
	.editor {
		min-width: 16rem;
		max-width: 28rem;

		min-height: 8rem;
		max-height: 14rem;

		overflow: auto;
		background-color: white;
		border-radius: 0.5rem;

		outline: 0px solid rgba(red, 0.5);
		transition: 75ms linear outline-width;

		&.error {
			outline-width: 3px;
		}
	}
</style>
