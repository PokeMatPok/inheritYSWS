<script lang="ts">
	import { goto } from '$app/navigation';
	import { resolve } from '$app/paths';
	import { onMount } from 'svelte';
	import { dev } from '$app/environment';

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

	function handleJoin(useEmail = true) {
		// in development, should not run in prod
		if (email && useEmail) {
			window.location.href = dev ?'http://localhost:3000/auth/login?email=' + encodeURIComponent(email) : 'https://inherit.dino.icu/api/auth/login?email=' + encodeURIComponent(email);
		} else {
			window.location.href = dev ? 'http://localhost:3000/auth/login' : 'https://inherit.dino.icu/api/auth/login';
		}


		// deprecated, kept for reference, should not run in prod
		/*
		if (email && useEmail) {
			window.open(
				`https://forms.fillout.com/t/mVAGqpQTbEus?embed=0&email=${encodeURIComponent(email)}`,
				'_blank'
			);
		} else {
			window.open('https://forms.fillout.com/t/mVAGqpQTbEus?embed=0', '_blank');
		}*/
	}

	onMount(() => {
		// check auth, redirect to /home if authd
		fetch(dev ? 'http://localhost:3000/auth/check' : 'https://inherit.dino.icu/api/auth/check', {
			credentials: 'include',
		})
			.then((res) => {
				return res.json();
			})
			.then((data) => {
				if (data.authenticated) {
					goto(resolve('/home'));
				}
			})
			.catch((err) => {
				console.error('Error checking auth:', err);
			});

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

		/*
		return () => {
			window.removeEventListener('scroll', handleScroll);
		};
		*/
	});
</script>

<a href="https://hackclub.com/"
	><img
		style="position: absolute; top: 0; left: 10px; border: 0; width: 256px; z-index: 999;"
		src="https://assets.hackclub.com/flag-orpheus-top.svg"
		alt="Hack Club"
	/></a
>

<div class="manual-sticky">
	<a href={resolve('/manual')}>Read the Manual</a>
</div>

<div>
	<div
		style="position: absolute; top: 330px; left: 370px; border: 0; width: 160px; z-index: 999; transform: scaleX(-1);"
	>
		<img src="/star.svg" alt="Star Left" />
	</div>
</div>

<div>
	<div
		style="position: absolute; top: 420px; right: 340px; border: 0; width: 170px; z-index: 999; transform: scaleX(-1) rotate(180deg);"
	>
		<img src="/star.svg" alt="Star Right" />
	</div>
</div>

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
					<button class="cta" onclick={() => handleJoin()}> RSVP <span hidden> post release note: need to replace RSVP with Join</span></button>
				</div>
				<button hidden class="demo">Watch Demo</button>
			</div>
		</div>
	</div>
	<div class="hero-tiles"></div>
</section>

<section class="page">
	<div>
		<div class="top">
			<h2>What's Inherit?</h2>
		</div>
		<div class="bottom">
			<div class="image-wrapper">
				<img
					src="/about.svg"
					alt="Orpheus questioning Inherit"
					class="explain-image"
					style="transform: translateY(-10%);"
				/>
			</div>

			<div class="page-content">
				<h3>Reviving other projects</h3>
				<p>
					Someone built their project at 2am with excitement. Then the hackathon ended, school
					started again, and the repo went quiet. But the code and the idea of the author are still
					there. Inherit gives abandoned Hack Club projects a second life and rewards the teen who
					brings them back.
				</p>
			</div>
		</div>
	</div>
</section>

<section class="page">
	<div>
		<div class="bottom prize-bottom">
			<div class="page-content">
				<h3>What do you get?</h3>
				<p>
					By contributing to those projects you not only get the satisfaction of reviving a project
					but also some cool rewards from Hack Club.
				</p>
			</div>

			<div class="prize-counter">
				<div class="carousel-item">
					<div class="carousel-image-wrapper">
						<img src="/badger.png" alt="Github Badger" class="carousel-image" />
					</div>
					<div class="carousel-content">
						<h4>Github Badger 2350</h4>
						<p>
							Your personal Hackable Conference Badge. This will definitely make you the coolest
							hacker around!
						</p>
						<span class="requirements">
							10+ hours of tracked time &middot; 1+ meaningful contribution
						</span>
					</div>
				</div>

				<div class="carousel-item">
					<div class="carousel-image-wrapper">
						<img src="/cards.png" alt="Github Cards" class="carousel-image" />
					</div>
					<div class="carousel-content">
						<h4>Github Cards</h4>
						<p>Some cool cards for your next Poker Night.</p>
						<span class="requirements">
							5+ hours of tracked time &middot; 1+ meaningful contribution
						</span>
					</div>
				</div>

				<div class="carousel-item">
					<div class="carousel-image-wrapper">
						<img
							src="/working_in_public.jpg"
							alt="Working in Public Cover"
							class="carousel-image"
						/>
					</div>
					<div class="carousel-content">
						<h4>Working in Public</h4>
						<p>
							An exciting book about the history and modern culture of open source, written by Nadia
							Asparouhova.
						</p>
						<span class="requirements">
							5+ hours of tracked time &middot; 1+ meaningful contribution
						</span>
					</div>
				</div>

				<div class="carousel-item">
					<div class="carousel-image-wrapper">
						<img src="/stickers.png" alt="Github Cards" class="carousel-image" />
					</div>
					<div class="carousel-content">
						<h4>Hack Club Stickers</h4>
						<p>
							Some cool stickers to decorate your laptop and show off your open source contributions
							to the world.
						</p>
						<span class="requirements"> 2+ hours of tracked time </span>
					</div>
				</div>

				<div class="carousel-item">
					<div class="carousel-image-wrapper">
						<img src="/charity.svg" alt="Charity Icon" class="carousel-image" />
					</div>
					<div class="carousel-content">
						<h4>Donate to Charity</h4>
						<p>
							Spend your prize on something good. We will donate the equivalent amount of your prize
							to a charity of your choice found on <a
								href="https://www.every.org/search"
								target="_blank">Every.org</a
							>.
						</p>
						<span class="requirements">
							starting at 5+ hours &middot; 1+ meaningful contribution
						</span>
					</div>
				</div>
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
				<img alt="01 Icon" src="/01.svg" class="number-image" />
				<h3>Browse old projects</h3>
				<p>
					Search through our archive of abandoned projects. Find one that sparks your interest and
					get to know it.
				</p>
			</div>
		</div>
	</div>
</section>

<section class="page flip">
	<div>
		<div class="bottom">
			<div class="page-content">
				<img alt="02 Icon" src="/02.svg" class="number-image" />
				<h3>Claim your Project</h3>
				<p>
					Found something? Claim it. Set your prize goal upfront — the bigger the contribution, the
					better the reward.
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
				<img alt="03 Icon" src="/03.svg" class="number-image" />
				<h3>Build</h3>
				<p>
					Fork it. Understand it. Make it better. Track your time with Hackatime and build something
					the original author would be proud of.
				</p>
			</div>
		</div>
	</div>
</section>

<section class="page flip">
	<div>
		<div class="bottom">
			<div class="page-content">
				<img alt="04 Icon" src="/04.svg" class="number-image" />
				<h3>Submit & Review</h3>
				<p>
					Open a pull request. Peers who've worked on similar projects review your code — real
					feedback, real open source workflow.
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
				<img src="/prize.svg" alt="Prize Graphic" class="explain-image prize-image" />
			</div>
			<div class="page-content">
				<img alt="05 Icon" src="/05.svg" class="number-image" />
				<h3>Receive your prize</h3>
				<p>
					PR merged. Project lives again. Hack Club ships the prize you set as your goal on day one.
					You earned it.
				</p>
			</div>
		</div>
	</div>
</section>

<section class="page">
	<div>
		<div class="top">
			<h2>FAQ</h2>
			<p>(Forked And Questioned)</p>
		</div>
		<div class="bottom">
			<div class="image-wrapper">
				<img src="/faq.svg" alt="Frequently Asked Questions Graphic" class="explain-image" />
			</div>

			<div class="page-content">
				<details>
					<summary>Who can join?</summary>
					<p>
						Inherit is open to all Hack Club Members. Whether you're a seasoned coder or just
						starting out, if you have a passion for open source and a desire to learn, you're
						welcome to join.
					</p>
				</details>
				<details>
					<summary>What counts as a meaningful contribution?</summary>
					<p>
						Any contribution adding a real usable feature, rewriting large portions of the codebase,
						or reworking a core concept counts. Typo fixes, README edits, and minor style changes
						don't count — we're looking for work that moves the project forward.
					</p>
				</details>
				<details>
					<summary>What if the original author doesn't merge my PR?</summary>
					<p>
						If the original author does not merge your PR in a few days, we still value and evaluate
						your contribution the same, you will still receive your prize. The original author can
						still merge your changes later. We have a team of staff who will review your work
						independently.
					</p>
				</details>
				<details>
					<summary>Can I work in a language I'm still learning?</summary>
					<p>
						If you're still learning a language, our recommendation engine will match you with more
						beginner-friendly projects. We encourage you to challenge yourself, but we'll make sure
						the project is a good fit.
					</p>
				</details>
				<details>
					<summary>How does Hackatime work?</summary>
					<p>
						Hackatime helps you track your project time, it quietly runs in your IDE and tracks your
						coding time automatically. Install it once and forget about it.
					</p>
				</details>
				<details>
					<summary>When do I get my prize?</summary>
					<p>
						You get your prize shipped as soon as you complete your goal, we will try to cover all
						taxes but there may still arise some taxes to specific countries, that we simply can not
						cover. Shipping is free worldwide.
					</p>
				</details>
				<div>
					<span>Other questions?</span>
					<a href={resolve('/manual#faq')}>Check the manual</a>
					<span>or</span>
					<a href="https://hackclub.enterprise.slack.com/user/@U0A2L4NMA9X" target="_blank"
						>Reach out</a
					>
				</div>
			</div>
		</div>
	</div>
</section>

<footer>
	<div class="footer-content">
		<a href="https://hackclub.com/"
			><img
				style="position: relative; top: 0; left: 0; border: 0; width: 256px; z-index: 999;"
				src="https://assets.hackclub.com/flag-orpheus-left.svg"
				alt="Hack Club"
			/></a
		>
		<div class="footer-cta">
			<img src="/inherit-logo.svg" alt="Inherit Logo" />
			<h2>Rethink. Rebuild. Reship.</h2>
			<span>(and repeat)</span>

			<div class="buttons">
				<div>
					<button class="cta" onclick={() => handleJoin(false)}> RSVP <span hidden> post release note: need to replace RSVP with Join</span> </button>
					<button onclick={() => goto(resolve('/manual'))}> Read the Manual </button>
				</div>
			</div>
		</div>
		<div style="flex: 0.2;"></div>
	</div>
	<div
		style="text-align: center; padding: 2rem; color: var(--text-muted); font-family: var(--font);"
	>
		Inherit is a Hack Club YSWS &middot; Made with &#10084; by <a
			href="https://hackclub.enterprise.slack.com/user/@U0A2L4NMA9X"
			target="_blank">Matthias Schreiber</a
		>
		&middot; <a href="https://github.com/PokeMatPok/inheritYSWS" target="_blank">Github</a>
	</div>

	<div class="footer-tiles"></div>
</footer>

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

	section {
		min-height: 100vh;
		display: flex;
		justify-content: space-between;
		align-items: center;

		filter: blur(0);
		transition: all 1.5s cubic-bezier(0.22, 1, 0.36, 1);
	}

	section:global(.intersection) {
		filter: blur(0);
	}

	// hero
	.hero {
		background-color: var(--bg);
		position: relative;
		overflow: hidden;
		height: 100vh;

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

		.hero-tiles {
			position: absolute;
			bottom: 0;
			left: 0;
			right: 0;
			height: 50px;
			background: url('/footer-tile.svg') repeat-x;
			background-size: auto 100%;
			opacity: 0.6;
			z-index: 999;
		}

		.hero-content {
			display: flex;
			flex-direction: column;
			align-items: center;
			width: 100%;
			justify-content: center;
			position: relative;
			z-index: 1;
			color: var(--text);

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
					color: var(--text-muted);
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

	button {
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

	// sections
	section:not(.hero) {
		background-color: var(--bg);
		border-top: 1px solid rgba(126, 71, 31, 0.15);

		&:nth-child(even) {
			background-color: var(--bg-alt);
		}
	}

	.page {
		width: 100%;
		padding: 0 10vw;
		box-sizing: border-box;
		display: flex;
		flex-direction: column;
		gap: 2rem;
		justify-content: center;
		padding-top: 0;
		align-items: center;
		background: url('/bg.svg') center / cover no-repeat;

		&:nth-child(5) {
			background: url('/bg_smiley.svg') center / cover no-repeat !important;
		}

		&:nth-child(7) {
			background: url('/bg_comment.svg') center / cover no-repeat !important;
		}

		&:nth-child(11) {
			background: url('/bg_faq.svg') center / cover no-repeat !important;
		}

		> div {
			width: 100%;
			height: 100%;
			min-height: calc(120vh - 8vh);
			display: flex;
			flex-direction: column;
			align-items: center;
			justify-content: space-between;
			padding-top: 0;
		}

		.top {
			width: 70%;
			padding: 6vh 4rem 4vh;
			display: flex;
			justify-content: center;
			align-items: center;
			flex-direction: column;
			position: relative;
			background: url('/box2.svg') center / 100% 100% no-repeat;
			margin: 0 auto;
			align-self: stretch;

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
			padding: 4vh 0 6vh;
			flex: 1;
			align-items: center;
		}

		.prize-bottom {
			padding-top: 20px;
			flex-direction: column;
		}

		.number-image {
			width: 100px;
			height: 100px;
			margin-bottom: 1rem;
		}

		.image-wrapper {
			flex: 1;
			display: flex;
			justify-content: center;
			align-items: center;
			min-width: 260px;

			.explain-image {
				width: 100%;
				max-width: 600px;
				border: 0;
			}

			.prize-image {
				position: absolute;
				bottom: 0;
				height: 90vh;
				padding-top: 20vh;
				width: unset !important;
				max-width: unset !important;
				border: 0;
				margin-top: 2rem;
				@media (min-width: 768px) {
					margin-top: 0;
				}
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
				font-size: clamp(2rem, 4vw, 2.8rem);
				margin: 0;
				color: var(--brown-light);
			}

			p {
				font-size: 1.6rem;
				margin: 0;
				color: var(--text-muted);
				line-height: 1.7;
				max-width: 480px;
			}

			span {
				font-size: 1rem;
				color: var(--text-muted);
				margin-right: 0.5rem;
			}

			a {
				font-size: 1rem;
				color: var(--brown-mid);
				text-decoration: none;
				padding: 0 5px;

				&:hover {
					text-decoration: underline;
				}
			}

			details {
				width: 100%;
				max-width: 600px;
				border: solid 2px var(--brown-dark);
				border-radius: 10px;
				padding: 1rem;
				background-color: transparent;
				color: var(--text);
				font-size: 1.1rem;

				summary {
					cursor: pointer;
					outline: none;
					font-weight: bold;
					color: var(--brown-mid);
				}

				p {
					margin-top: 0.5rem;
					font-size: 1rem;
					color: var(--text-muted);
				}
			}
		}

		&.flip .bottom {
			flex-direction: row-reverse;
		}

		.prize-counter {
			display: flex;
			align-items: center;
			gap: 1rem;
			margin-top: 2rem;
			display: flex;
			flex-wrap: wrap;
			justify-content: center;
			gap: 1.5rem;
			max-width: 900px;
			margin: 0 auto;

			.carousel-item {
				display: flex;
				flex-direction: column;
				align-items: center;
				background: url('/boxCarousel.svg') center / 100% 100% no-repeat;
				width: 400px;
				height: 500px;
				box-sizing: border-box;
				gap: 1rem;
				padding: 1rem 2rem;

				flex: 0 0 calc(33% - 1rem); // 3 per row
				min-width: 220px;

				.carousel-image-wrapper {
					width: 150px;
					height: 150px;
					border-radius: 50%;
					overflow: hidden;
					display: flex;
					justify-content: center;
					align-items: center;
					margin-top: 50px;

					.carousel-image {
						width: 100%;
						height: auto;
						border-radius: 50%;
						border: solid 4px var(--brown-dark);
						background: #fff;
					}
				}

				.carousel-content {
					text-align: center;

					h4 {
						font-size: 1.5rem;
						margin: 0;
						color: var(--brown-mid);
					}

					p {
						font-size: 1rem;
						margin: 0;
						color: var(--text-muted);
						max-width: 200px;
					}
				}
			}
		}
	}

	footer {
		background-color: var(--bg);
		border-top: 1px solid rgba(126, 71, 31, 0.15);
		padding-top: 4vh;

		.footer-tiles {
			position: relative;
			bottom: 0;
			left: 0;
			right: 0;
			height: 50px;
			background: url('/footer-tile.svg') repeat-x;
			background-size: auto 100%;
			opacity: 0.6;
			z-index: 0;
		}

		.footer-content {
			display: flex;
			justify-content: space-between;
			align-items: center;
			box-sizing: border-box;
			position: relative;
			z-index: 1;

			.footer-cta {
				display: flex;
				flex-direction: column;
				align-items: center;
				gap: 0.5rem;

				img {
					width: clamp(200px, 20vw, 260px);
				}

				h2 {
					font-size: 1.5rem;
					margin: 0;
					color: var(--text);
				}

				span {
					font-size: 1rem;
					color: var(--text-muted);
				}
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
				}
			}
		}
	}

	.manual-sticky {
		position: fixed;
		bottom: 1.5rem;
		right: 1.5rem;
		z-index: 999;

		a {
			font-family: var(--font);
			background-color: var(--brown-mid);
			color: var(--text);
			border-radius: 30px;
			padding: 0.8rem 1.5rem;
			font-size: 0.95rem;
			text-decoration: none;
			box-shadow: 3px 3px 0px var(--brown-dark);
			transition: transform 0.15s ease;
			display: block;

			&:hover {
				transform: translateY(-2px);
			}
		}
	}
</style>
