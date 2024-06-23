<script>
	let dragging = $state(false);

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
	<div
		class="preview"
		ondragover={(e) => {
			if (e.dataTransfer) {
				e.preventDefault();
				e.dataTransfer.effectAllowed = 'move';
			}
		}}
	>
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

							transition-property: background-color, inset;
						}

						&.dragging {
							&::after {
								inset: -0.5rem;
								background-color: rgba(blue, 0.1);
							}
						}
					}
				}
			}
		}
	}
</style>
