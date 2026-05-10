<script lang="ts">
	import { goto } from '$app/navigation';
	import { page } from '$app/state';
	import { resolve } from '$app/paths';

	let displayLine = $state('');

	function getRandomLine(lines: string[]) {
		return lines[Math.floor(Math.random() * lines.length)];
	}

	switch (page.status) {
		case 404:
			displayLine = getRandomLine([
				'We looked everywhere, but this page is playing hard to get.',
				"This page ghosted us. It's not you, it's… actually, we're not sure.",
				'Looks like this page swiped left on existence.'
			]);
			break;
		case 500:
			displayLine = getRandomLine([
				"Our server is having a moment. It's not you, it's us.",
				"Looks like our server tripped over a wire. We're on it!",
				"Our server is taking an unexpected coffee break. We'll get it back to work soon!",
				'The server tried its best, but your request was simply too much to handle.',
				'Our server got flustered and needs a moment to collect itself.'
			]);
			break;
		case 403:
			displayLine = getRandomLine([
				'This page is out of your league. (Access denied.)',
				'Nice try, but this page has very high standards.'
			]);
			break;
		default:
			displayLine = getRandomLine([
				"Something went wrong, but we're not sure what. It's a mystery!",
				'An unexpected error occurred. Our team of highly trained monkeys is on the case.',
				"Well, this is awkward. Something went wrong, but we're not sure what."
			]);
	}
</script>

<div class="bg">
	<img src="/error.svg" alt="Burning server graphic" style="width: 200px; height: auto;" />
	<h1>{page.status}: {page.error?.message}</h1>
	<p>{displayLine}</p>
	<p>Still on fire? Reach out to support.</p>
	<div class="buttons">
		<button onclick={() => goto(resolve('/'))}>Go Home</button>
		<button onclick={() => location.reload()}>Try Again</button>
	</div>
</div>

<style>
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
	}

	.bg {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		height: 100vh;
		width: 100vw;
		text-align: center;
		background-color: #c29a72;
		color: #1a0f05;
		font-family: 'Phantom Sans', sans-serif;
		margin: 0;
		padding: 0;
	}

	.buttons {
		display: flex;
		gap: 1rem;
		flex-wrap: wrap;
		justify-content: center;
		align-items: center;
		padding: 2rem 10vw;

		button {
			box-shadow: 3px 3px 0px var(--brown-dark);
			transition:
				background-color 0.15s ease,
				border-color 0.15s ease,
				color 0.15s ease;
			margin: 0 10px;
			font-family: var(--font);
			background-color: var(--brown-mid);
			border: solid 2px var(--brown-mid);
			color: var(--text);
			border-radius: 30px;
			font-size: 1rem;
			padding: 0.5rem 1.2rem;
			cursor: pointer;
			box-shadow: 3px 3px 0px var(--brown-dark);
			margin: 0 10px;
			transition:
				background-color 0.15s ease,
				border-color 0.15s ease,
				color 0.15s ease;

			&:hover {
				background-color: var(--brown-light);
				border-color: var(--brown-light);
				color: #1a0f05;
			}
		}
	}
</style>
