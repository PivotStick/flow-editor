<script>
	/**
	 * @type {import("@xyflow/svelte").NodeProps}
	 */
	let { data } = $props();

	/**
	 * @type {{ text: string; }[]}
	 */
	let buttons = $state([]);

	/**
	 * @param {DragEvent} e
	 */
	function onDrop(e) {
		buttons.push({
			text: 'Action'
		});
	}

	let dragging = $state(false);
</script>

<svelte:window ondrop={() => (dragging = false)} />

<!-- svelte-ignore a11y_no_static_element_interactions -->
<div class="container" ondragleave={() => (dragging = false)} ondragover={() => (dragging = true)}>
	<div class="options">
		<span>Capsule State</span>
		<select value={data.state} disabled>
			<option value="unclaimed">Not Claimed</option>
			<option value="claimed">Claimed</option>
		</select>

		<span>Persona</span>
		<select value={data.persona} disabled>
			<option value="any">Any</option>
			<option value="owner">Owner</option>
			<option value="anonymous">Anonymous</option>
			<option value="user">User</option>
		</select>
	</div>

	<div class="screen">
		<img src="https://cdn.skatepro.com/product/520/radio-legion-26-wheelie-bike-bg.webp" alt="" />
		<div class="sheet">
			<div class="actions" class:dragging ondrop={onDrop} dropzone="move">
				{#each buttons as button}
					<button>
						<span contenteditable="true" bind:textContent={button.text} />
					</button>
				{/each}
			</div>
		</div>
	</div>
</div>

<style lang="scss">
	.options {
		padding: 2rem;
		display: grid;
		grid-template-columns: auto 1fr;
		gap: 0.5rem;
	}

	.screen {
		aspect-ratio: 9 / 19.5;
		border-radius: 1.75rem;
		height: 85dvh;
		border: 3px solid rgba(black, 0.25);
		background-color: #fafafa;

		display: flex;
		flex-direction: column;
		overflow: hidden;

		img {
			aspect-ratio: 1;
			width: 80%;
			object-fit: contain;
			align-self: center;
			margin-top: 1.35rem;
		}

		.sheet {
			flex: 1;
			background-color: white;
			height: 100%;
			box-shadow: 0 -0.5rem 2rem -1rem rgba(black, 0.5);
			border-radius: 1rem 1rem 0 0;

			position: relative;

			.actions {
				z-index: 0;
				position: absolute;
				top: 0;
				left: 1rem;
				right: 1rem;
				min-height: 1rem;
				translate: 0 -50%;

				display: flex;
				align-items: center;
				gap: 0.5rem;

				button {
					flex: 1;
				}

				&::after {
					content: '';
					z-index: -1;
					inset: -0.25rem;
					position: absolute;
					border-radius: 0.5rem;
					outline: 0px solid rgba(var(--color-500) / 90%);

					transition-property: background-color, inset, outline-width;
				}

				&.dragging {
					&::after {
						inset: -0.5rem;
						background-color: rgba(var(--color-500) / 30%);
					}

					&:hover {
						&::after {
							outline-width: 3px;
						}
					}
				}
			}
		}
	}
</style>
