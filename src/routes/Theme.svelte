<script>
	import { onMount } from 'svelte';

	// Function to get a cookie by name
	const getCookie = (name) => {
		const value = `; ${document.cookie}`;
		const parts = value.split(`; ${name}=`);
		if (parts.length === 2) return parts.pop().split(';').shift();
	};

	// Function to set a cookie
	const setCookie = (name, value, days) => {
		const expires = new Date(Date.now() + days * 864e5).toUTCString();
		document.cookie = `${name}=${value}; expires=${expires}; path=/`;
	};

	let theme = 'light';

	// Check for saved theme in cookie and apply it
	onMount(() => {
		const savedTheme = getCookie('theme');
		if (savedTheme) {
			theme = savedTheme;
			document.body.classList.add(theme);
		} else {
			document.body.classList.add('light');
		}
	});

	// Toggle theme and update cookie
	const toggleTheme = () => {
		theme = theme === 'light' ? 'dark' : 'light';
		document.body.classList.toggle('light');
		document.body.classList.toggle('dark');
		setCookie('theme', theme, 30); // Save theme for 30 days
	};
</script>

<button on:click={toggleTheme}>
	Switch to {theme === 'light' ? 'Dark' : 'Light'} Theme
</button>

<style>
	body {
		transition: background-color 0.3s ease;
	}
	body.light {
		background-color: white;
		color: black;
	}
	body.dark {
		background-color: #121212;
		color: white;
	}

	button {
		padding: 10px;
		border: none;
		cursor: pointer;
		font-size: 16px;
		background-color: #007bff;
		color: white;
		border-radius: 5px;
	}

	button:hover {
		background-color: #0056b3;
	}
</style>
