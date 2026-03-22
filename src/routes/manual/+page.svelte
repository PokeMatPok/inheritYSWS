<script lang="ts">
	// No JS needed for this page
</script>

<div class="manual-wrapper">
	<!-- Top nav -->
	<nav class="top-nav">
		<a href="/" class="back-btn">← Back to Inherit</a>
	</nav>

	<!-- Hero -->
	<header class="manual-header">
		<img src="/inherit-logo.svg" alt="Inherit" class="manual-logo" />
		<h1>Inherit Manual</h1>
		<p class="manual-subtitle">
			An in-depth documentation of the Inherit YSWS, concept, design, and implementation.
		</p>
		<div class="manual-links">
			<a href="https://www.notion.so/Inherit-32304b1f6061800b9ec6d3cbc5ee3b30" target="_blank" class="manual-link">Read additional Notes on Notion ↗</a>
		</div>
	</header>

	<!-- TOC -->
	<aside class="toc">
		<div class="toc-label">Contents</div>
		<ol>
			<li><a href="#introduction">Introduction</a></li>
			<li><a href="#concept">Concept</a></li>
			<li><a href="#design">Design</a></li>
			<li><a href="#implementation">Implementation</a></li>
			<li><a href="#sources">Sources</a></li>
		</ol>
	</aside>

	<!-- Content -->
	<article class="manual-content">

		<section id="introduction">
			<h2>Introduction</h2>
			<p>
				Open source software is everywhere, most of the products and services we use daily are built
				on top of it. In 2024, GitHub alone reported 5.2 billion contributions across 518 million
				open source projects.<sup><a href="#src-1">[1]</a></sup> But despite this scale, many
				projects are quietly dying. Survival rates for open source repositories have been falling
				steadily, not just obscure ones, but even well-known projects are losing activity,
				maintainers, and community support.
			</p>
			<figure>
				<img src="/manual/survival_rate_decline.png" alt="Survival Analysis per Ecosystem" />
				<figcaption>
					Source: <a href="https://livablesoftware.com/survival-rate-github-projects-empirical/" target="_blank">
						Survival Rate of GitHub Projects: An Empirical Study
					</a>
				</figcaption>
			</figure>
			<p>
				Why? Contributing to open source is intimidating. Even experienced developers struggle to
				find the right project, understand an unfamiliar codebase, or navigate the social dynamics
				of a public PR. For beginners, it can feel impossible.
			</p>
			<p>
				Inherit is a YSWS (You Ship, We Ship) program that aims to fix this, by giving Hack Club
				members a structured, supported, and rewarded path into open source contribution. Because
				collaborative building and learning from each other is at the core of what Hack Club is
				about.
			</p>
		</section>

		<section id="concept">
			<h2>Concept</h2>
			<p>
				Inherit is built for Hack Club members who want to contribute to open source but don't know
				where to start. The core idea: find an abandoned project, make it better, open a PR, and get
				rewarded. Not just for the code, but for the experience of actually doing it.
			</p>

			<h3>Why "Inherit"?</h3>
			<p>
				The dictionary definition of inherit is "to receive or be left with a situation or object
				from a predecessor." That's exactly what this program is about. You inherit a project from
				someone who built it, got busy, and moved on. You work on it, leave your mark, and pass it
				forward for the next person.
			</p>

			<h3>Tracking Contributions</h3>
			<p>
				Contributions are tracked using Hackatime, which runs quietly in the contributor's editor
				and logs time automatically. Every 2–3 hours of tracked time, a Slack bot checks in and
				asks for a short summary of what was built, keeping a transparent record without disrupting
				the workflow. These check-ins are opt-out, and contributors can always submit summaries
				manually through the dashboard instead.
			</p>

			<h3>Rewards and Recognition</h3>
			<p>
				Contributors set their prize goal upfront, before they start working. The bigger the
				commitment, the better the reward. Current prizes include the GitHub Badger 2350 hackable
				badge, GitHub playing cards, the book <em>Working in Public</em> by Nadia Asparouhova, and
				Hack Club stickers. Contributors can also choose to donate the equivalent value to a charity
				of their choice via Every.org instead of receiving a physical prize.
			</p>

			<h3>Project Recommendation System</h3>
			<p>
				Finding the right project to contribute to is half the battle. Inherit's recommendation
				engine matches contributors to projects based on their tech stack, interests, and experience
				level, gathered through an optional onboarding form that can be updated at any time.
				Contributors can also browse and filter the full project index manually, and are free to
				work on any project they choose regardless of what gets recommended.
			</p>
			<p>
				Projects are indexed through a combination of automated scraping of past Hack Club
				hackathon submissions and manual submissions from project authors. The index includes
				metadata like language, difficulty level, and last activity date, helping contributors find
				something that fits.
			</p>
		</section>

		<section id="design">
			<h2>Design</h2>

			<h3>Design Philosophy</h3>
			<p>
				Inherit isn't a SaaS. It's a community program with a human story behind every project in
				its index. A clean, minimal design would contradict that, it would make Inherit feel like a
				tool rather than a place. The warmth of the hand-drawn aesthetic mirrors the warmth of the
				concept itself: something built by people, for people.
			</p>
			<p>
				Every design decision was made to feel like it belongs in the same world as the
				illustrations, rough edges, warm colors, things that look made rather than generated.
			</p>

			<h3>Color Palette</h3>
			<p>The entire site uses four shades derived from the logo:</p>
			<ul class="color-list">
				<li>
					<span class="color-dot" style="background-color: #FFBF8F;"></span>
					<code>#FFBF8F</code>, light brown, warm highlights and glows
				</li>
				<li>
					<span class="color-dot" style="background-color: #AA5F27;"></span>
					<code>#AA5F27</code>, mid brown, buttons, accents, headings
				</li>
				<li>
					<span class="color-dot" style="background-color: #7E471F;"></span>
					<code>#7E471F</code>, dark brown, borders, shadows
				</li>
				<li>
					<span class="color-dot" style="background-color: #3d2008;"></span>
					<code>#3D2008</code>, near-black brown, primary text
				</li>
			</ul>
			<p>
				These weren't chosen arbitrarily, they came directly out of the logo. The background is a
				warm paper tone rather than pure white or black, which keeps everything feeling cohesive
				and grounded.
			</p>

			<h3>Logo Design</h3>
			<div class="logo-compare">
				<div>
					<img src="/inherit-logo.svg" alt="Current logo" />
					<p>Current version</p>
				</div>
				<div>
					<img src="/manual/logo-old.svg" alt="Earlier version" />
					<p>Earlier version</p>
				</div>
			</div>
			<p>
				The logo is a wooden plaque, plaques commemorate things, they mark legacy and history.
				Orpheus peeks over the top, watching a small plant sprout from the wood. The plant is the
				key symbol: something abandoned growing again. The hand-drawn lettering is intentionally
				imperfect, matching the overall aesthetic.
			</p>
			<p>
				The earlier version had Orpheus sitting on top of the plaque with a rougher, sketchier
				style. The current version adds shading, a cleaner composition, and the sprout detail that
				ties the revival metaphor together.
			</p>

			<h3>Illustrations</h3>
			<p>
				Every illustration on the site was drawn from scratch in Figma, no stock art, no
				AI-generated images, no third-party libraries. The hand-drawn style was a deliberate choice
				to contrast with the generic SaaS aesthetic that dominates most YSWS sites.
			</p>
			<div class="illustration-compare">
				<div>
					<img src="/about.svg" alt="Inherit FAQ illustration" />
					<p>Inherit's hand-drawn style</p>
				</div>
				<div>
					<img src="/manual/undraw_faq.svg" alt="Generic undraw illustration" />
					<p>Generic undraw.co equivalent</p>
				</div>
			</div>

			<h3>Typography</h3>
			<p>
				Inherit uses Phantom Sans, Hack Club's own custom typeface. Using it was a deliberate
				choice to feel native to the Hack Club ecosystem rather than importing a generic font.
				Phantom Sans has rounded edges and an informal character that complements the hand-drawn
				illustrations without competing with them.
			</p>
		</section>

		<section id="implementation">
			<h2>Implementation</h2>

			<h3>Webpage</h3>
			<p>
				The site is built with SvelteKit and TypeScript, styled with SCSS. Vercel handles hosting
				and deploys automatically on every push to main. The custom domain <code>inherit.dino.icu</code>
				is configured via the <a href="https://github.com/hackclub/dns" target="_blank">hackclub/dns
				GitHub repository</a>.
			</p>

			<h3>Forms and RSVP</h3>
			<p>
				RSVPs are handled through Fillout. When a user types their email on the landing page and
				hits Join, they're redirected to the Fillout form with their email pre-filled, one less
				thing to type.
			</p>

			<h3>Contribution Tracking</h3>
			<p>
				Hackatime runs in the contributor's editor and tracks coding time automatically, no manual
				logging. Every 2–3 hours a Slack bot checks in for a short update. These check-ins can be
				turned off at any time. Time data determines prize eligibility.
			</p>

			<h3>Charity Donations</h3>
			<p>
				The charity option runs through Every.org, which vets every nonprofit on its platform.
				Contributors pick any listed organization, the donation is processed via Hack Club Bank,
				and the contributor gets a receipt and a personal thank you via Slack.
			</p>

			<h3>Design and Illustration</h3>
			<p>
				All visuals, logo, step illustrations, section dividers, step numbers, background textures,
				UI frames, were designed and drawn in Figma and exported as SVGs. Everything is original.
			</p>
		</section>

		<section id="sources">
			<h2>Sources</h2>
			<ol class="sources-list">
				<li id="src-1">
					<a href="https://electroiq.com/stats/open-source-statistics/" target="_blank">
						Open Source Statistics, electroiq.com
					</a>
				</li>
				<li>
					<a href="https://livablesoftware.com/survival-rate-github-projects-empirical/" target="_blank">
						Survival Rate of GitHub Projects: An Empirical Study, livablesoftware.com
					</a>
				</li>
			</ol>
		</section>

	</article>

	<!-- Bottom nav -->
	<div class="bottom-nav">
		<a href="/" class="back-btn">← Back to Inherit</a>
	</div>

</div>

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
			url('https://assets.hackclub.com/fonts/Phantom_Sans_0.7/Bold.woff') format('woff'),
			url('https://assets.hackclub.com/fonts/Phantom_Sans_0.7/Bold.woff2') format('woff2');
		font-weight: bold;
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

	:global(body) {
		margin: 0;
		padding: 0;
		background-color: #c29a72;
		background-image: url('/bg.svg');
		background-attachment: fixed;
		background-size: cover;
		font-family: 'Phantom Sans', sans-serif;
		color: #3d2008;
	}

	// ── Nav ───────────────────────────────────────────────────
	.top-nav, .bottom-nav {
		max-width: 760px;
		margin: 0 auto;
		padding: 1.5rem 2rem;
	}

	.back-btn {
		display: inline-block;
		font-family: 'Phantom Sans', sans-serif;
		font-size: 0.95rem;
		color: #7e471f;
		text-decoration: none;
		border: 2px solid #7e471f;
		border-radius: 30px;
		padding: 0.4rem 1rem;
		transition: all 0.2s ease;

		&:hover {
			background: #7e471f;
			color: #ffbf8f;
		}
	}

	// ── Header ────────────────────────────────────────────────
	.manual-header {
		max-width: 760px;
		margin: 0 auto;
		padding: 2rem 2rem 3rem;
		text-align: center;
		border-bottom: 2px solid rgba(126, 71, 31, 0.2);

		.manual-logo {
			width: clamp(180px, 30vw, 280px);
			margin-bottom: 1.5rem;
			filter: drop-shadow(0 4px 16px rgba(126, 71, 31, 0.3));
		}

		h1 {
			font-size: clamp(2rem, 5vw, 3rem);
			margin: 0 0 0.75rem;
			color: #3d2008;
		}

		.manual-subtitle {
			font-size: 1.05rem;
			color: #7e471f;
			margin: 0 0 1.5rem;
			max-width: 480px;
			margin-left: auto;
			margin-right: auto;
			line-height: 1.6;
		}

		.manual-links {
			display: flex;
			gap: 1rem;
			justify-content: center;
			flex-wrap: wrap;
		}

		.manual-link {
			font-family: 'Phantom Sans', sans-serif;
			font-size: 0.9rem;
			color: #aa5f27;
			text-decoration: none;
			border-bottom: 2px solid rgba(170, 95, 39, 0.4);
			padding-bottom: 2px;
			transition: border-color 0.2s;

			&:hover {
				border-color: #aa5f27;
			}
		}
	}

	// ── TOC ───────────────────────────────────────────────────
	.toc {
		max-width: 760px;
		margin: 2rem auto;
		padding: 1.25rem 2rem;
		background: rgba(126, 71, 31, 0.08);
		border-left: 4px solid #aa5f27;
		border-radius: 0 8px 8px 0;

		.toc-label {
			font-size: 0.7rem;
			letter-spacing: 0.12em;
			text-transform: uppercase;
			color: #aa5f27;
			font-weight: bold;
			margin-bottom: 0.75rem;
		}

		ol {
			margin: 0;
			padding-left: 1.2rem;
			display: flex;
			flex-direction: column;
			gap: 0.4rem;
		}

		a {
			color: #7e471f;
			text-decoration: none;
			font-size: 0.95rem;

			&:hover {
				color: #3d2008;
				text-decoration: underline;
			}
		}
	}

	// ── Article ───────────────────────────────────────────────
	.manual-content {
		max-width: 760px;
		margin: 0 auto;
		padding: 0 2rem 4rem;

		section {
			margin-bottom: 4rem;
			padding-bottom: 3rem;
			border-bottom: 1px solid rgba(126, 71, 31, 0.15);

			&:last-child {
				border-bottom: none;
			}
		}

		h2 {
			font-size: clamp(1.6rem, 3vw, 2.2rem);
			color: #3d2008;
			margin: 0 0 1.5rem;
			padding-bottom: 0.5rem;
			border-bottom: 3px solid #aa5f27;
			display: inline-block;
		}

		h3 {
			font-size: 1.2rem;
			color: #7e471f;
			margin: 2rem 0 0.75rem;
		}

		p {
			font-size: 1rem;
			line-height: 1.8;
			color: #4a2e14;
			margin: 0 0 1rem;
		}

		a {
			color: #aa5f27;
			text-decoration: underline;
			text-underline-offset: 3px;
		}

		code {
			font-family: 'Courier New', monospace;
			background: rgba(126, 71, 31, 0.1);
			padding: 0.1em 0.4em;
			border-radius: 4px;
			font-size: 0.9em;
			color: #7e471f;
		}

		sup a {
			color: #aa5f27;
			text-decoration: none;
			font-size: 0.75em;
		}

		figure {
			margin: 1.5rem 0;

			img {
				width: 100%;
				border-radius: 8px;
				border: 2px solid rgba(126, 71, 31, 0.2);
			}

			figcaption {
				font-size: 0.8rem;
				color: #7e471f;
				margin-top: 0.5rem;
				font-style: italic;
			}
		}

		img:not(figure img):not(.manual-logo) {
			max-width: 100%;
			border-radius: 8px;
		}
	}

	// ── Color list ────────────────────────────────────────────
	.color-list {
		list-style: none;
		padding: 0;
		margin: 1rem 0;
		display: flex;
		flex-direction: column;
		gap: 0.5rem;

		li {
			display: flex;
			align-items: center;
			gap: 0.75rem;
			font-size: 0.95rem;
			color: #4a2e14;
		}
	}

	:global(.color-dot) {
		display: inline-block;
		width: 18px;
		height: 18px;
		border-radius: 50%;
		border: 2px solid rgba(61, 32, 8, 0.2);
		flex-shrink: 0;
	}

	// ── Logo compare ──────────────────────────────────────────
	.logo-compare, .illustration-compare {
		display: flex;
		gap: 2rem;
		margin: 1.5rem 0;
		flex-wrap: wrap;

		> div {
			flex: 1;
			min-width: 200px;
			text-align: center;

			img {
				width: 100%;
				max-width: 280px;
				border-radius: 8px;
			}

			p {
				font-size: 0.8rem;
				color: #7e471f;
				font-style: italic;
				margin-top: 0.5rem;
			}
		}
	}

	// ── Sources ───────────────────────────────────────────────
	.sources-list {
		padding-left: 1.5rem;
		display: flex;
		flex-direction: column;
		gap: 0.5rem;

		li {
			font-size: 0.9rem;
			color: #4a2e14;
		}
	}

	// ── Bottom nav ────────────────────────────────────────────
	.bottom-nav {
		border-top: 1px solid rgba(126, 71, 31, 0.15);
		text-align: center;
		padding: 2rem;
	}
</style>