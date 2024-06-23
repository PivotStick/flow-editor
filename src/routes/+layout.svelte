<script>
	import { SvelteFlowProvider } from '@xyflow/svelte';
	import '../app.scss';
	import '@xyflow/svelte/dist/style.css';

	let { children } = $props();
</script>

<SvelteFlowProvider>
	{@render children()}
</SvelteFlowProvider>
