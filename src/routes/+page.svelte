<script lang="ts">
	import { onMount } from 'svelte';

	let scrollTop = $state(0);

	let scrollHeight = 0;
	let clientHeight = 0;
	let scrollPercent = $derived((scrollTop / (scrollHeight - clientHeight)) * 100);
	let email = $state('');

	$effect(() => {
		const heroElement: HTMLElement | null = document.querySelector('.hero');
		if (heroElement) {
			heroElement.style.setProperty('--scroll-percent', `${scrollPercent}%`);
		}
	});

	$effect(() => {
		if (scrollTop > 0) {
			const el = document.querySelector('.orpheus-image') as HTMLElement;

			const percent = Math.min(scrollPercent, 100);

			if (scrollTop <= clientHeight) {
				el?.style.setProperty(
					'--scroll-animation-value-bottom',
					`${percent * 0.01 * clientHeight}px`
				);

				el?.style.setProperty('--scroll-animation-value-right', `${percent * 0.02 * 500}px`);

				el?.style.setProperty('--scroll-animation-value-scale', `${percent * 0.02 + 1}`);
			}
		}
	});

	function handleJoin() {
		if (email) {
			window.open(
				`https://forms.fillout.com/t/mVAGqpQTbEus?embed=0&email=${encodeURIComponent(email)}`,
				'_blank'
			);
		} else {
			window.open('https://forms.fillout.com/t/mVAGqpQTbEus?embed=0', '_blank');
		}
	}

	onMount(() => {
		scrollHeight =
			document.documentElement.scrollHeight || document.querySelector('section')?.scrollHeight || 0;

		clientHeight = document.documentElement.clientHeight || window.innerHeight;
		const handleScroll = () => {
			scrollTop = document.documentElement.scrollTop || document.body.scrollTop;
		};

		window.addEventListener('scroll', handleScroll);

		const observer = new IntersectionObserver(
			(entries) => {
				entries.forEach((entry) => {
					if (entry.isIntersecting) {
						entry.target.classList.add('intersection');
					} else {
						entry.target.classList.remove('intersection');
					}
				});
			},
			{ threshold: 0.2 }
		);

		document.querySelectorAll('section').forEach((section) => {
			observer.observe(section);
		});

		handleScroll();

		return () => {
			window.removeEventListener('scroll', handleScroll);
		};
	});
</script>

<a href="https://hackclub.com/"
	><img
		style="position: absolute; top: 0; left: 10px; border: 0; width: 256px; z-index: 999;"
		src="https://assets.hackclub.com/flag-orpheus-top.svg"
		alt="Hack Club"
	/></a
>

<section class="hero">
	<div class="hero-content">
		<img
			src="/inherit-logo.svg"
			alt="Inherit"
			style="width: 500px; margin-bottom: 1rem;z-index: 999;"
		/>
		<div class="tag">
			<h1 class="highlight">Rethink</h1>
			<h1>Rebuild</h1>
			<h1>Reship</h1>
		</div>
		<div class="sub">
			<div>Take an abandoned project. Make it yours. Ship it.</div>
			<div class="buttons">
				<div>
					<input type="email" placeholder="Enter your email" class="cta-input" bind:value={email} />
					<button class="cta" onclick={handleJoin}> Join </button>
				</div>
				<button hidden class="demo">Watch Demo</button>
			</div>
		</div>
	</div>
</section>

<section class="page">
	<div>
		<div class="top">
			<h2>How It Works</h2>
		</div>
		<div class="bottom">
			<div class="image-wrapper">
				<img src="/browse.svg" alt="Browse Graphic" class="explain-image" />
			</div>

			<div class="page-content">
				<h3>Browse old projects</h3>
				<p>
					Search through our archive of abandoned projects. Find one that sparks your interest and get to know it.
				</p>
			</div>
		</div>
	</div>
</section>

<section class="page flip">
	<div>
		<div class="bottom">
			<div class="page-content">
				<h3>Claim it</h3>
				<p>
					Found something? Claim it. Set your prize goal upfront — the bigger the contribution, the better the reward.
				</p>
			</div>
			<div class="image-wrapper">
				<img src="/claim.svg" alt="Claim Graphic" class="explain-image" />
			</div>
		</div>
	</div>
</section>

<section class="page">
	<div>
		<div class="bottom">
			<div class="image-wrapper">
				<img src="/build.svg" alt="Build Graphic" class="explain-image" />
			</div>
			<div class="page-content">
				<h3>Build</h3>
				<p>
					Fork it. Understand it. Make it better. Track your time with Hackatime and build something the original author would be proud of.
				</p>
			</div>
		</div>
	</div>
</section>

<section class="page flip">
	<div>
		<div class="bottom">
			<div class="page-content">
				<h3>Submit & Review</h3>
				<p>
					Open a pull request. Peers who've worked on similar projects review your code — real feedback, real open source workflow.
				</p>
			</div>
			<div class="image-wrapper">
				<img src="/submit.svg" alt="Submit Graphic" class="explain-image" />
			</div>
		</div>
	</div>
</section>

<section class="page">
	<div>
		<div class="bottom">
			<div class="image-wrapper">
				<img src="/prize.svg" alt="Prize Graphic" class="explain-image" />
			</div>
			<div class="page-content">
				<h3>Receive your prize</h3>
				<p>
					PR merged. Project lives again. Hack Club ships the prize you set as your goal on day one. You earned it.
				</p>
			</div>
		</div>
	</div>
</section>

<style lang="scss">
	@font-face {
		font-family: 'Phantom Sans';
		src:
			url('https://assets.hackclub.com/fonts/Phantom_Sans_0.7/Regular.woff') format('woff'),
			url('https://assets.hackclub.com/fonts/Phantom_Sans_0.7/Regular.woff2') format('woff2');
		font-weight: normal;
		font-style: normal;
		font-display: swap;
	}
	@font-face {
		font-family: 'Phantom Sans';
		src:
			url('https://assets.hackclub.com/fonts/Phantom_Sans_0.7/Italic.woff') format('woff'),
			url('https://assets.hackclub.com/fonts/Phantom_Sans_0.7/Italic.woff2') format('woff2');
		font-weight: normal;
		font-style: italic;
		font-display: swap;
	}
	@font-face {
		font-family: 'Phantom Sans';
		src:
			url('https://assets.hackclub.com/fonts/Phantom_Sans_0.7/Bold.woff') format('woff'),
			url('https://assets.hackclub.com/fonts/Phantom_Sans_0.7/Bold.woff2') format('woff2');
		font-weight: bold;
		font-style: normal;
		font-display: swap;
	}

	:root {
		--bg: #0d0b09;
		--bg-alt: #120f0c;
		--brown-dark: #7e471f;
		--brown-mid: #aa5f27;
		--brown-light: #ffbf8f;
		--text: #f5e6d0;
		--text-muted: #a08060;
		--font: 'Phantom Sans', sans-serif;
	}

	:global(body) {
		margin: 0;
		padding: 0;
		background-color: var(--bg);
		color: var(--text);
		font-family: var(--font);
	}

	section {
		min-height: 100vh;
		display: flex;
		justify-content: space-between;
		align-items: center;

		filter: blur(5px);
		transition: all 1.5s cubic-bezier(0.19, 1, 0.22, 1);
	}

	section:global(.intersection) {
		filter: blur(0);
	}

	// ── Hero ──────────────────────────────────────────────────
	.hero {
		background-color: var(--bg);
		position: relative;
		overflow: hidden;

		&::after {
			content: '';
			position: absolute;
			bottom: -80px;
			left: 50%;
			transform: translateX(-50%);
			width: 500px;
			height: 260px;
			background: radial-gradient(ellipse, rgba(170, 95, 39, 0.2) 0%, transparent 70%);
			pointer-events: none;
		}

		.hero-content {
			display: flex;
			flex-direction: column;
			align-items: center;
			width: 100%;
			justify-content: center;
			position: relative;
			z-index: 1;

			.tag {
				font-family: var(--font);
				font-size: 2.5rem;
				width: min-content;

				display: flex;
				flex-direction: column;
				align-items: start;
				justify-content: center;
				gap: 0.1rem;
				margin: 0;
				padding: 0;

				* {
					margin: 0;
					padding: 0;
					color: var(--text);
				}

				.highlight {
					position: relative;
					display: inline-block;
					color: var(--brown-light);
				}

				.highlight::before {
					content: '';
					position: absolute;
					inset: -0.2em -0.4em;
					background: url('/highlight.svg') center / 100% 100% no-repeat;
					z-index: -1;
					opacity: 0.5;
				}
			}

			.sub {
				font-family: var(--font);
				font-size: 1.1rem;
				margin-top: 1rem;
				display: flex;
				flex-direction: column;
				align-items: center;
				gap: 1rem;
				color: var(--text-muted);

				button {
					font-family: var(--font);
					background-color: var(--brown-mid);
					border: solid 2px var(--brown-mid);
					color: var(--text);
					border-radius: 30px;
					font-size: 1rem;
					padding: 0.5rem 1.2rem;
					cursor: pointer;

					&:hover {
						background-color: var(--brown-light);
						border-color: var(--brown-light);
						color: #1a0f05;
					}
				}

				.cta-input {
					font-family: var(--font);
					border: solid 2px var(--brown-dark);
					color: var(--text);
					background-color: transparent;
					border-radius: 30px;
					font-size: 1rem;
					padding: 0.5rem 1rem;
					margin-right: 0.5rem;
					width: 250px;

					&::placeholder {
						color: var(--text-muted);
					}

					&:focus {
						outline: none;
						border-color: var(--brown-mid);
					}
				}
			}
		}
	}

	.hero::before {
		content: '';
		position: absolute;
		inset: 0;
		background: url('/bg_highlight.svg') center / cover no-repeat;
		opacity: 0.06;
		z-index: 0;
		background-position: 0 var(--scroll-percent, 0%);
	}

	.orpheus-image {
		position: absolute;
		bottom: var(--scroll-animation-value-bottom, 0);
		scale: clamp(1, var(--scroll-animation-value-scale, 1), 2);
		right: var(--scroll-animation-value-right, 10px);
		border: 0;
		width: clamp(400px, 40vw, 700px);
		z-index: 999;
	}

	// ── Step sections ─────────────────────────────────────────
	section:not(.hero) {
		background-color: var(--bg);
		border-top: 1px solid rgba(126, 71, 31, 0.15);

		&:nth-child(even) {
			background-color: var(--bg-alt);
		}

		&:nth-of-type(2) {
			border-top-left-radius: 20px;
			border-top-right-radius: 20px;
			border: solid 1px var(--brown-dark);
			border-bottom: none;
		}
	}

	.page {
		width: 100%;
		padding: 0 10vw;
		box-sizing: border-box;
		display: flex;
		flex-direction: column;
		gap: 2rem;
		justify-content: space-around;
		align-items: center;

		> div {
			width: 100%;
		}

		.top {
			width: 100%;
			padding: 6vh 0 2vh;
			display: flex;
			justify-content: center;
			align-items: center;

			h2 {
				font-size: clamp(1.8rem, 4vw, 3rem);
				color: var(--text);
				margin: 0;
				position: relative;

				&::after {
					content: '';
					display: block;
					width: 2.5rem;
					height: 3px;
					background: var(--brown-mid);
					margin-top: 0.5rem;
					border-radius: 2px;
				}
			}
		}

		.bottom {
			width: 100%;
			display: flex;
			justify-content: space-around;
			align-items: center;
			gap: 4rem;
			flex-wrap: wrap;
			box-sizing: border-box;
			padding-bottom: 6vh;
		}

		.image-wrapper {
			flex: 1;
			display: flex;
			justify-content: center;
			align-items: center;
			min-width: 260px;

			.explain-image {
				width: 100%;
				max-width: 460px;
				border: 0;
			}
		}

		.page-content {
			flex: 1;
			min-width: 260px;
			font-family: var(--font);
			display: flex;
			flex-direction: column;
			justify-content: center;
			align-items: flex-start;
			gap: 1rem;

			h3 {
				font-size: clamp(1.4rem, 3vw, 2.2rem);
				margin: 0;
				color: var(--brown-light);
			}

			p {
				font-size: 1.1rem;
				margin: 0;
				color: var(--text-muted);
				line-height: 1.7;
				max-width: 480px;
			}
		}

		&.flip .bottom {
			flex-direction: row-reverse;
		}
	}
</style>