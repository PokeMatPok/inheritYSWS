<script lang="ts">
	import { onMount } from 'svelte';

	let contentElement: HTMLDivElement;

	type ConfettiFn = (options: {
		particleCount: number;
		spread: number;
		origin: { y: number };
	}) => void;

	onMount(() => {
		if (!contentElement) return;

		const handleAnimationEnd = () => {
			setTimeout(() => {
				const confettiFn = (window as Window & { confetti?: ConfettiFn }).confetti;
				if (!confettiFn) return;

				confettiFn({
					particleCount: 100,
					spread: 70,
					origin: { y: 0.6 }
				});
			}, 500);
		};

		contentElement.addEventListener('animationend', handleAnimationEnd);

		return () => {
			contentElement.removeEventListener('animationend', handleAnimationEnd);
		};
	});
</script>

<svelte:head>
	<script
		src="https://cdn.jsdelivr.net/npm/@tsparticles/confetti@3.9.1/tsparticles.confetti.bundle.min.js"
	></script>
</svelte:head>

<div>

	<img
		src="/sent.svg"
		class="fly-animation"
		alt="Sent graphic"
		style="width: 200px; height: auto;"
	/>

	<div class="content" bind:this={contentElement}>
		<div class="content-inner">
			<img src="/inherit-logo.svg" alt="Inherit logo" style="width: 200px; height: auto;" />

			<div class="text">
				<h1>You're In!</h1>
				<p>You're on the waitlist!</p>
				<p>We'll notify you on launch. In the meantime, check your DMs</p>
				<p>Orpheus should have sent you a welcome message!</p>
			</div>
		</div>
	</div>
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
		overflow: hidden;
	}

	.fly-animation {
		position: absolute;
		top: 50%;
		left: 50%;
		transform: translate(-50%, -50%);

		animation: fly 3.6s linear forwards;
	}

	.content {
		background: repeating-linear-gradient(
				105deg,
				#b77a48 0 6px,
				#bc8050 6px 12px,
				#b37545 12px 17px,
				#c18655 17px 23px
			)
			padding-box;
		border: 10px solid transparent;
		box-shadow:
			inset 0 0 0 1px rgba(36, 20, 8, 0.28),
			inset 0 10px 18px rgba(255, 222, 180, 0.12),
			inset 0 -12px 20px rgba(48, 25, 10, 0.2);
		clip-path: polygon(
			6% 8%,
			14% 4%,
			28% 2%,
			44% 5%,
			58% 3%,
			73% 5%,
			88% 3%,
			95% 10%,
			98% 23%,
			96% 38%,
			99% 54%,
			97% 70%,
			94% 86%,
			87% 96%,
			72% 98%,
			56% 96%,
			41% 99%,
			25% 97%,
			11% 99%,
			4% 90%,
			2% 76%,
			4% 60%,
			1% 45%,
			3% 28%,
			5% 15%
		);
		padding: 20px;
		transform: translateY(100vh);
		animation: appear 1s ease-out 3.6s forwards;
	}

	.content-inner {
		background: linear-gradient(var(--brown-light), var(--brown-light)) padding-box;
		border: 8px solid transparent;
		box-shadow:
			inset 0 0 0 1px rgba(63, 33, 13, 0.2),
			inset 0 8px 14px rgba(255, 235, 210, 0.2),
			inset 0 -10px 14px rgba(90, 50, 21, 0.16);
		clip-path: polygon(
			8% 9%,
			16% 6%,
			29% 4%,
			44% 7%,
			57% 5%,
			72% 6%,
			86% 5%,
			92% 11%,
			95% 23%,
			93% 37%,
			96% 52%,
			94% 68%,
			91% 84%,
			85% 92%,
			71% 94%,
			56% 92%,
			42% 95%,
			27% 93%,
			14% 95%,
			8% 88%,
			6% 75%,
			8% 60%,
			5% 46%,
			7% 30%,
			8% 18%
		);
		padding: 40px;

		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		text-align: center;
	}

	// fly animation, bottom left to top right with subtle left-right drift
	@keyframes fly {
		0% {
			transform: translate(-44vw, 64vh) rotate(-10deg);
		}
		10% {
			transform: translate(-36vw, 53vh) rotate(4deg);
		}
		20% {
			transform: translate(-30vw, 41vh) rotate(-3deg);
		}
		30% {
			transform: translate(-23vw, 30vh) rotate(5deg);
		}
		40% {
			transform: translate(-17vw, 17vh) rotate(-2deg);
		}
		50% {
			transform: translate(-10vw, 4vh) rotate(4deg);
		}
		60% {
			transform: translate(-5vw, -10vh) rotate(-2deg);
		}
		68% {
			transform: translate(-8vw, -20vh) rotate(3deg);
		}
		76% {
			transform: translate(-2vw, -31vh) rotate(-2deg);
		}
		84% {
			transform: translate(-4vw, -54vh) rotate(2deg);
		}
		92% {
			transform: translate(4vw, -73vh) rotate(-1deg);
		}
		99% {
			transform: translate(9vw, -100vh) rotate(0deg);

			opacity: 1;
		}
		100% {
			transform: translate(10vw, -110vh) rotate(0deg);

			// hide the element after the animation completes to prevent it from being visible during confetti
			opacity: 0;
		}
	}
	@keyframes appear {
		0% {
			transform: translateY(100vh);
		}
		100% {
			transform: translateY(0);
		}
	}
</style>
