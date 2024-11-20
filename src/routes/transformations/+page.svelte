<script>
	let transformations = $state([]);
	import Loader from '../Loader.svelte';
	import Back from '../Back.svelte';
	setTimeout(() => {
		transformations = JSON.parse(localStorage.getItem('transformations'));
	}, 3000);

	let body = $state('hidden');
	let loader = $state('visible');

	$effect(() => {
		setTimeout(() => {
			body = 'visible';
			loader = 'hidden';
		}, 3000);
	});
</script>

<main class="container-fluid">
	<div style="visibility: {loader};">
		<Loader />
	</div>
	<div id="body" style="visibility: {body};">
		<h1>Transformations</h1>
		<Back />
		<div id="transformations">
			{#each transformations as transformation}
				<div class="zoom">
					<img class="img-fluid" src={transformation.image} alt={transformation.name} />
					<p>{transformation.name}</p>
					<p>Ki: {transformation.ki}</p>
					<br />
				</div>
			{/each}
		</div>
	</div>
</main>

<style>
	p {
		font-size: large;
	}

	h1 {
		color: teal;
		text-align: center;
	}

	main {
		padding: 0px 50px;
	}

	#transformations {
		/* display: grid; */

		grid-template-columns: repeat(2, 1fr);
	}

	@media (min-width: 769px) {
		#transformations {
			display: grid;
			grid-template-columns: repeat(4, 1fr);
		}
	}

	@media (max-width: 769px) {
		.zoom img {
			max-width: 12rem;
		}

		#transformations {
			display: inline-flexbox;
		}
	}

	.zoom {
		padding: 0px;
		max-width: 32rem;
		display: inline-block;
		margin: auto;
		transition: transform 0.2s; /* Animation */
	}

	.zoom:hover {
		transform: scale(
			1.2
		); /* (150% zoom - Note: if the zoom is too large, it will go outside of the viewport) */
	}
</style>
