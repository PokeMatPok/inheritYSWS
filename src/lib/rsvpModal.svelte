<script lang="ts">
	import { dev } from '$app/environment';
	import { onMount } from 'svelte';
	import StylizedBox from './stylizedBox.svelte';

	let userEmail: string | undefined;

	let showRsvpModal = $state(false);

	export function openRsvpModal(enteredEmail?: string) {
		userEmail = enteredEmail;
		showRsvpModal = true;

		console.log('Opening RSVP modal with email:', userEmail);
	}

	export function closeRsvpModal() {
		showRsvpModal = false;
	}

	function handleJoin(method = 'hackclub' as 'hackclub' | 'fillout') {
		if (method === 'hackclub') {
			if (userEmail) {
				window.location.href = dev
					? 'http://localhost:3000/auth/login?email=' + encodeURIComponent(userEmail)
					: 'https://inherit.dino.icu/api/auth/login?email=' + encodeURIComponent(userEmail);
			} else {
				window.location.href = dev
					? 'http://localhost:3000/auth/login'
					: 'https://inherit.dino.icu/api/auth/login';
			}
		} else {
			if (userEmail) {
				window.open(
					`https://forms.fillout.com/t/mVAGqpQTbEus?embed=0&email=${encodeURIComponent(userEmail)}`,
					'_blank'
				);
			} else {
				window.open('https://forms.fillout.com/t/mVAGqpQTbEus?embed=0', '_blank');
			}
		}
	}

	onMount(() => {
		document.addEventListener("keydown", (event) => {
			if (event.key === "Escape" && showRsvpModal) {
				closeRsvpModal();
			}
		});
	})
</script>

{#if showRsvpModal}
	<img src="/rsvp.svg" alt="RSVP illustration" class="rsvp-illustration" />

	<button
		type="button"
		class="backdrop"
		aria-label="Close RSVP modal"
		onclick={() => closeRsvpModal()}
	></button>

	<StylizedBox class="rsvp-modal">
		<img src="/inherit-logo.svg" alt="Inherit graphic" class="logo">
		<p>RSVP with</p>
		<button onclick={() => handleJoin('hackclub')} class="hc-link">
			<span class="hc-link-text"> Hackclub </span>

			<img
				src="https://assets.hackclub.com/icon-rounded.svg"
				alt="hackclub logo icon"
				class="hc-link-icon"
			/>
		</button>
		or
		<button onclick={() => handleJoin('fillout')} class="fillout-link">
			<span class="fillout-link-text"> Fillout </span>
			<img src="/branded/fillout-icon.svg" alt="Fillout logo icon" class="fillout-link-icon" />
		</button>
	</StylizedBox>
{/if}

<style lang="scss">
	.logo {
		width: 200px;
		height: auto;
		margin-bottom: 20px;
	}

	.rsvp-illustration {
		position: fixed;
		bottom: 50vh;
		right: 50vw;
		transform: translate(150%, 0);
		width: 250px;
		height: auto;
		z-index: 1001;
	}

	.hc-link,
	.fillout-link {
		border: none;
		padding: 15px;
		margin: 5px;
		border-radius: 20px;
		display: inline-flex;
		align-items: center;
		font-family: 'Phantom Sans', sans-serif;
		font-size: 20px;
		cursor: pointer;
	}

	.hc-link {
		background-color: #ec3750;
		color: white;
	}

	.hc-link-icon {
		width: 20px;
		height: 20px;
		margin-left: 8px;
	}

	.fillout-link {
		background-color: #f0f0f0;
		color: #333;
	}

	.fillout-link-icon {
		width: 20px;
		height: 20px;
		margin-left: 8px;
	}

	.backdrop {
		position: fixed;
		top: 0;
		left: 0;
		width: 100vw;
		height: 100vh;
		background-color: rgba(0, 0, 0, 0.5);
		z-index: 999;
		animation: reveal 0.3s ease-out forwards;
	}

	@keyframes reveal {
		from {
			opacity: 0;
		}
		to {
			opacity: 1;
		}
	}

	:global(.rsvp-modal) {
		position: fixed;
		top: 50%;
		left: 50%;
		transform: translate(-50%, -50%);
		z-index: 1000;
		animation: reveal 0.3s ease-out forwards;
	}
</style>
