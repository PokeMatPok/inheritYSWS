<script lang="ts">
	import { onMount } from 'svelte';

	onMount(() => {
		console.log('Home page mounted');
	});
</script>

<div>
	<h1>Welcome to the Home Page</h1>
</div>

<style lang="scss">
	@font-face {
		font-family: 'Phantom Sans';
		src:
			url('/Phantom_Sans_0.7_Bold.woff') format('woff'),
			url('/Phantom_Sans_0.7_Bold.woff2') format('woff2');
		font-weight: normal;
		font-style: normal;
		font-display: swap;
	}
    
	:root {
		--bg: #c29a72;
		--bg-alt: #6b5035;
		--brown-dark: #3d2008;
		--brown-mid: #bb723b;
		--brown-light: #fcc296;
		--text: #1a0f05;
		--text-muted: #2e1c0c;
		--font: 'Phantom Sans', sans-serif;
	}

	:global(body) {
		margin: 0;
		padding: 0;
		background-color: var(--bg);
		color: var(--text);
		font-family: var(--font);
	}
</style>
