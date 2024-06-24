<script>
	import { SvelteFlowProvider } from '@xyflow/svelte';
	import Flow from './Flow.svelte';

	let dragging = $state(false);
</script>

<main ondrop={() => (dragging = false)}>
	<aside>
		<button
			draggable="true"
			ondragstart={(e) => {
				if (e.dataTransfer) {
					e.dataTransfer.setData('application/text', 'stuff');
					e.dataTransfer.effectAllowed = 'move';
					dragging = true;
				}
			}}
		>
			Action
		</button>
	</aside>
	<!-- svelte-ignore a11y_no_static_element_interactions -->
	<div
		class="preview"
		ondragover={(e) => {
			if (e.dataTransfer) {
				e.preventDefault();
				e.dataTransfer.effectAllowed = 'move';
			}
		}}
	>
		<SvelteFlowProvider>
			<Flow />
		</SvelteFlowProvider>
	</div>
</main>

<style lang="scss">
	main {
		display: grid;
		grid-template-columns: auto 1fr;
		height: 100dvh;

		aside {
			border-right: 1px solid rgba(black, 0.25);
			padding: 1rem;
		}

		.preview {
			display: flex;
			align-items: center;
			justify-content: center;
			background-color: #fafafa;
		}
	}
</style>
