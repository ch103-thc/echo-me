<script>
	import '../main.scss';

	let { children } = $props();
</script>

<div class="terminal-window">
	<div class="terminal-header">
		<span class="btn red"></span>
		<span class="btn yellow"></span>
		<span class="btn green"></span>
		<span class="title">charlotte/portfolio</span>
	</div>
	<div class="terminal-body">
		{@render children()}
	</div>
</div>

<style lang="scss">
	.terminal-window {
		max-width: 50rem;
		margin: 3rem auto;
		border-radius: 0.5rem;
		box-shadow: 0 0 20px rgba(0, 0, 0, 0.6);
		background-color: var(--darker);
		overflow: hidden;

		.terminal-header {
			display: flex;
			align-items: center;
			padding: 0.5rem;
			background-color: var(--lighter);
			position: relative;

			.btn {
				width: 0.75rem;
				height: 0.75rem;
				border-radius: 50%;
				margin-right: 0.5rem;

				&.red {
					background-color: var(--red);
				}
				&.yellow {
					background-color: var(--orange);
				}
				&.green {
					background-color: var(--green);
				}
			}

			.title {
				color: var(--title);
				margin-left: auto;
				margin-right: auto;
				font-size: 0.9rem;
				position: absolute;
				left: 50%;
				transform: translateX(-50%);
			}
		}

		.terminal-body {
			padding: 1rem;
		}
	}

	@media (max-width: 640px) {
		.terminal-window {
			max-width: 90%;
			margin: 2rem auto;
		}
	}
</style>
