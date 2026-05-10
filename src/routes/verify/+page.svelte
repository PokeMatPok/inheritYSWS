<script lang="ts">
	import { dev } from '$app/environment';
	import { goto } from '$app/navigation';
	import { resolve } from '$app/paths';
	import StylizedBox from '$lib/stylizedBox.svelte';
	import { onMount } from 'svelte';

	let hasError = $state(false);
	let success = $state(false);
	let errorMessage = $state('');

	onMount(() => {
		const token = new URLSearchParams(window.location.search).get('token');

		if (!token) {
			return;
		}

		const url = dev ? 'http://localhost:3000/projects/verify' : '/api/projects/verify';

		const urlWithToken = `${url}?token=${encodeURIComponent(token)}`;

		fetch(urlWithToken, {
			method: 'POST',
			headers: {
				'Content-Type': 'application/json'
			}
		})
			.then((res) => {
				if (!res.ok) {
					hasError = true;
					return res.json();
				}

				success = true;
			})
			.then((data) => {
				if (hasError) {
					errorMessage = data.error || 'Verification failed. Please try again.';
					throw new Error(errorMessage);
				}
			})
			.catch((error) => {
				console.error('Error during verification:', error);
				hasError = true;
				errorMessage = error.message || 'An unexpected error occurred. Please try again.';
				// goto(resolve('/submitProject'));
			});
	});
</script>

<title>Submit Project - Inherit</title>
<div>
	<StylizedBox class="box">
		{#if success}
			<img
				src="/mail_logo.svg"
				alt="Inherit Logo"
				style="width: 200px; height: auto; margin-bottom: 20px;"
			/>
		{:else if hasError}
			<img
				src="/error.svg"
				alt="Error illustration"
				style="width: 150px; height: auto; margin-bottom: 20px;"
			/>
		{/if}

		<div class="text">
			{#if success}
				<h1>You are verified!</h1>
				<p>Thanks for submitting your project to Inherit,</p>
				<p>You are what makes Inherit great!</p>
				<p>We'll review it as soon as possible.</p>
			{:else if hasError}
				<h1>{errorMessage}</h1>
				<p>This really shouldn't have happened. If the problem persists, please contact support.</p>
				<p>We're sorry for your trouble.</p>
			{:else}
				<h1>Please wait...</h1>
				<p>Verification is in progress. Please do not close this page.</p>
			{/if}

			<div class="buttons">
				<button onclick={() => goto(resolve('/'))}>Return to Inherit</button>
			</div>
		</div>
	</StylizedBox>
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

		display: flex;
		justify-content: center;
		align-items: center;

		height: 100vh;
		width: 100vw;

		background: url('/bg.svg');
		overflow: hidden !important;
	}

	:global(.box) {
		animation: float-in 0.5s ease-out forwards;
	}

	.buttons {
		display: flex;
		gap: 1rem;
		justify-content: center;
		align-items: end;
		padding: 2rem 10vw;
		width: 100%;

		button {
			box-shadow: 3px 3px 0px var(--brown-dark);
			transition:
				background-color 0.15s ease,
				border-color 0.15s ease,
				color 0.15s ease;
			font-family: var(--font);
			background-color: var(--brown-mid);
			border: solid 2px var(--brown-mid);
			color: var(--text);
			border-radius: 30px;
			font-size: 1rem;
			padding: 0.5rem 1.2rem;
			cursor: pointer;
			box-shadow: 3px 3px 0px var(--brown-dark);
			margin: 0 30px;
		}
	}

	@keyframes float-in {
		from {
			opacity: 0;
			transform: translateY(20px);
		}

		to {
			opacity: 1;
			transform: translateY(0);
		}
	}

	@keyframes float {
		0% {
			transform: translateY(0);
		}

		50% {
			transform: translateY(-10px);
		}

		100% {
			transform: translateY(0);
		}
	}
</style>
