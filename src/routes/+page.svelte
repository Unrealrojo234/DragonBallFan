<script>
	import Character from './Character.svelte';
	import Loader from './Loader.svelte';
	import Theme from './Theme.svelte';

	let loader = $state('visible');
	let body = $state('hidden');

	$effect(() => {
		setTimeout(() => {
			loader = 'hidden';
			body = 'visible';
		}, 3000);
	});
</script>

<main class="container-fluid">
	<div style="visibility:{loader};" id="loader">
		<Loader />
	</div>
	<div id="body" style="visibility: {body};">
		<!-- <Theme /> -->
		<Character />
	</div>
</main>

<style>
	@media (max-width: 481px) {
		#loader {
			position: absolute;
			top: 20%;
			left: 16%;
		}
	}

	@media (min-width: 481px) {
		#loader {
			position: absolute;
			top: 20%;
			left: 70%;
		}
	}
</style>
