<script>
	import { error } from '@sveltejs/kit';
	let characters = $state([]);
	let userSearch = $state('');

	let api = 'https://dragonball-api.com/api/characters?limit=58';

	$effect(() => {
		fetch(api)
			.then((res) => res.json())
			.then((data) => {
				characters = data.items;
			})
			.catch((error) => console.log('Error ', error));
	});

	let transformations = $state([]); //will carry array of transformations

	let show = $state('none');

	//Fetching data for characters transformations
	function handleClick(id) {
		fetch(`https://dragonball-api.com/api/characters/${id}`)
			.then((res) => res.json())
			.then((data) => {
				console.log(data.transformations);
				transformations = data.transformations;
				if (data.transformations.length > 0) {
					show = 'block';
				}
			})
			.catch((error) => console.log('Error ', error));
	}
</script>

<main>
	<h1>Dragon Ball Fans</h1>
	<br />
	<div id="mySearch">
		<div class="searchBox">
			<input
				class="searchInput"
				bind:value={userSearch}
				type="text"
				name=""
				placeholder="Search something"
			/>
			<!-- svelte-ignore a11y_consider_explicit_label -->
			<button class="searchButton" href="#">
				<svg
					xmlns="http://www.w3.org/2000/svg"
					width="29"
					height="29"
					viewBox="0 0 29 29"
					fill="none"
				>
					<g clip-path="url(#clip0_2_17)">
						<g filter="url(#filter0_d_2_17)">
							<path
								d="M23.7953 23.9182L19.0585 19.1814M19.0585 19.1814C19.8188 18.4211 20.4219 17.5185 20.8333 16.5251C21.2448 15.5318 21.4566 14.4671 21.4566 13.3919C21.4566 12.3167 21.2448 11.252 20.8333 10.2587C20.4219 9.2653 19.8188 8.36271 19.0585 7.60242C18.2982 6.84214 17.3956 6.23905 16.4022 5.82759C15.4089 5.41612 14.3442 5.20435 13.269 5.20435C12.1938 5.20435 11.1291 5.41612 10.1358 5.82759C9.1424 6.23905 8.23981 6.84214 7.47953 7.60242C5.94407 9.13789 5.08145 11.2204 5.08145 13.3919C5.08145 15.5634 5.94407 17.6459 7.47953 19.1814C9.01499 20.7168 11.0975 21.5794 13.269 21.5794C15.4405 21.5794 17.523 20.7168 19.0585 19.1814Z"
								stroke="white"
								stroke-width="3"
								stroke-linecap="round"
								stroke-linejoin="round"
								shape-rendering="crispEdges"
							></path>
						</g>
					</g>
					<defs>
						<filter
							id="filter0_d_2_17"
							x="-0.418549"
							y="3.70435"
							width="29.7139"
							height="29.7139"
							filterUnits="userSpaceOnUse"
							color-interpolation-filters="sRGB"
						>
							<feFlood flood-opacity="0" result="BackgroundImageFix"></feFlood>
							<feColorMatrix
								in="SourceAlpha"
								type="matrix"
								values="0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 127 0"
								result="hardAlpha"
							></feColorMatrix>
							<feOffset dy="4"></feOffset>
							<feGaussianBlur stdDeviation="2"></feGaussianBlur>
							<feComposite in2="hardAlpha" operator="out"></feComposite>
							<feColorMatrix type="matrix" values="0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0.25 0"
							></feColorMatrix>
							<feBlend mode="normal" in2="BackgroundImageFix" result="effect1_dropShadow_2_17"
							></feBlend>
							<feBlend mode="normal" in="SourceGraphic" in2="effect1_dropShadow_2_17" result="shape"
							></feBlend>
						</filter>
						<clipPath id="clip0_2_17">
							<rect
								width="28.0702"
								height="28.0702"
								fill="white"
								transform="translate(0.403503 0.526367)"
							></rect>
						</clipPath>
					</defs>
				</svg>
			</button>
		</div>
	</div>
	<div style="display: {show};">
		<div class="chars">
			{#each transformations as transformation}
				<div id="transformations">
					<img
						loading="lazy"
						class="img-fluid"
						id={transformation.name}
						src={transformation.image}
						alt={transformation.name}
					/>
					<p>{transformation.name}</p>
				</div>
			{/each}
		</div>
	</div>
	<a style="display: {show};" href="#transformations" id="showTrans">
		<button id="showBtn">{null}</button>
	</a>
	<div id="contents">
		{#await characters}
			<p>Loading ...</p>
		{:then}
			{#each characters as character, i}
				{#if character.name.includes(userSearch)}
					<div class="zoom" id="character-container">
						<a onclick={handleClick(character.id)} href={`#${character.id}`}>
							<img
								id={character.id}
								loading="lazy"
								class="img-fluid"
								src={character.image}
								alt={character.name + ' image'}
							/>
						</a>
						<p>Name: {character.name}</p>
						{#if parseInt(character.ki) > 0}
							<p>Ki: {character.ki}</p>

							<p>Max Ki: {character.maxKi}</p>
						{/if}
					</div>
				{/if}
			{/each}
		{/await}
	</div>

	<br />
</main>

<style>
	/* From Uiverse.io by OnlyCodeChannel */
	#mySearch {
		display: flex;
		justify-content: center;
		align-items: center;
		margin-bottom: 1px;
	}
	.searchBox {
		display: flex;
		max-width: 230px;
		align-items: center;
		justify-content: space-between;
		gap: 8px;
		background: #2f3640;
		border-radius: 50px;
		position: relative;
	}

	.searchButton {
		color: white;
		position: absolute;
		right: 8px;
		width: 50px;
		height: 50px;
		border-radius: 50%;
		background: var(--gradient-2, linear-gradient(90deg, #2af598 0%, #009efd 100%));
		border: 0;
		display: inline-block;
		transition: all 300ms cubic-bezier(0.23, 1, 0.32, 1);
	}
	/*hover effect*/
	button:hover {
		color: #fff;
		background-color: #1a1a1a;
		box-shadow: rgba(0, 0, 0, 0.5) 0 10px 20px;
		transform: translateY(-3px);
	}
	/*button pressing effect*/
	button:active {
		box-shadow: none;
		transform: translateY(0);
	}

	.searchInput {
		border: none;
		background: none;
		outline: none;
		color: white;
		font-size: 15px;
		padding: 24px 46px 24px 26px;
	}
	#showTrans {
		position: fixed;
		top: 50%;
		right: 3%;
	}
	h1 {
		text-align: center;
		color: teal;
	}

	.zoom {
		padding: 50px;
		transition: transform 0.2s; /* Animation */
	}

	.zoom:hover {
		transform: scale(
			1.5
		); /* (150% zoom - Note: if the zoom is too large, it will go outside of the viewport) */
	}

	@media (min-width: 769px) {
		#contents {
			display: grid;
			grid-template-columns: repeat(7, 1fr);
			grid-gap: 5px;
		}
	}
	@media (max-width: 769px) {
		#contents {
			max-width: 16rem;
		}
	}
	p {
		font-size: large;
	}

	#transformations {
		max-width: 32rem;
	}

	#transformations img {
		width: 18rem;
	}

	.chars {
		display: grid;
		grid-template-columns: repeat(2, 1fr);
		grid-gap: 5px;
	}

	#showBtn {
		width: 40px;
		height: 40px;
		border-radius: 50%;
		border: none;
		background-color: teal;
	}
</style>
