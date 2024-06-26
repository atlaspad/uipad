<script>
	import { onMount } from 'svelte';
	import Header from '../lib/partials/header.svelte';
	import calculatePadding from '../lib/utils/calculatePadding';
	import Footer from '../lib/partials/footer.svelte';

	let columns = 0;
	let rows = 0;
	let total = 1;

	const getGridSize = () => {
		columns = Math.floor(document.body.clientWidth / 50);
		rows = Math.floor(document.body.clientHeight / 50);
		total = rows * columns;
	};

	let main;
	onMount(() => {
		getGridSize();
		window.addEventListener('resize', getGridSize);
		window.addEventListener('resize', () => {
			calculatePadding(main, 'paddingInline');
		});

		calculatePadding(main, 'paddingInline');
	});
</script>

<svelte:head>
	<!-- we might move those font import declarations to app.html -->
	<link rel="preconnect" href="https://fonts.googleapis.com" />
	<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
	<link
		href="https://fonts.googleapis.com/css2?family=GFS+Neohellenic&family=Ubuntu:wght@400;500;700&display=swap"
		rel="stylesheet"
	/>
</svelte:head>

<div id="grid">
	{#each Array(total) as _, i}
		<div class="grid-item" id={i}></div>
	{/each}
</div>

<Header />

<main bind:this={main}>
	<slot></slot>
</main>

<Footer />

<style lang="scss">
	$square_size: 80px;

	:global(*) {
		margin: 0;
		padding: 0;
		scroll-behavior: smooth;
	}

	:global(html) {
		height: -webkit-fill-available;
		height: -moz-available;
		height: fill-available;
		height: 100%;
	}

	:global(body) {
		min-height: -webkit-fill-available;
		min-height: -moz-available;
		min-height: fill-available;
		height: 100%;

		color: #fff;
		background-color: #0f0f0f;
	}

	:global(hr) {
		border: none;
		height: 1px;
		background: linear-gradient(
			90deg,
			rgba(255, 255, 255, 0.5) 0%,
			#fff 50%,
			rgba(255, 255, 255, 0.5) 100%
		);
	}

	:global(.font-monospace) {
		font-family: 'Ubuntu Mono', monospace !important;
	}

	:global(.color-muted) {
		color: #ccc;
	}

	#grid {
		z-index: -1;
		position: fixed;
		top: 0;
		width: 100vw;
		height: 100svh;
		height: 100vh;

		display: grid;
		grid-template-columns: repeat(auto-fit, minmax($square_size, 1fr));
		grid-template-rows: repeat(auto-fit, minmax($square_size, 1fr));
		justify-content: center;

		background: linear-gradient(
			45deg,
			#4B04F140,
			#35ADA140,
			#52FF0040,
			#88F2E740,
			#79996040,
			#EAFF9640,
			#FF5B9640
		);
		background-size: 200% 200%;
		animation: flowingGradient 8s ease infinite;

		@keyframes flowingGradient {
			0% {
				background-position: 0% 50%;
			}
			50% {
				background-position: 100% 50%;
			}
			100% {
				background-position: 0% 50%;
			}
		}

		.grid-item {
			min-width: 100%;
			min-height: 100%;
			background-color: transparent;
			cursor: pointer;
			position: relative;
			&:after {
				content: '';
				position: absolute;
				background-color: rgb(15, 15, 15);
				inset: .5px;
			}
		}

		&:after {
			content: '';
			position: absolute;
			top: 0;
			left: 0;
			right: 0;
			bottom: 0;

			background-image: url('bg/reveal.png');
		}
	}
</style>
