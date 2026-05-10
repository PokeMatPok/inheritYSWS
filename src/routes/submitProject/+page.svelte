<script lang="ts">
	/* eslint-disable @typescript-eslint/no-unused-vars */

	import { dev } from '$app/environment';
	import StylizedBox from '$lib/stylizedBox.svelte';
	import { onMount } from 'svelte';
	import { type User } from '$lib/types.js';
	import { Octokit, type RestEndpointMethodTypes } from '@octokit/rest';
	import RsvpModal from '$lib/rsvpModal.svelte';

	const octokit = new Octokit();

	let authenticated: boolean | null = $state(null);
	let user: User | null = $state(null);
	let noHackclubLogin = $state(false);
	let noHackclubContact: string | null = $state(null);
	let activePage = $state('url');
	let continueEnabled = $state(false);
	let projectUrl = $state('');
	let isNotGithubDev = $state(false);
	let repoData: RestEndpointMethodTypes['repos']['get']['response'] | null = $state(null);
	let rsvpModalRef: RsvpModal | null = $state(null);

	let data: {
		name: string;
		projectName: string;
		description: string;
		url: string;
		notGithub: boolean;
		programmingLanguage: string;
		improvementPotential: string;
		difficulty: number;
		contactMethod: 'hackclub' | 'email' | 'slack';
		contactInfo: string;
	} = $state({
		name: '',
		projectName: '',
		description: '',
		url: '',
		notGithub: false,
		programmingLanguage: '',
		improvementPotential: '',
		difficulty: 1,
		contactMethod: 'hackclub',
		contactInfo: ''
	});

	const difficultyLabels = ['Very Easy', 'Easy', 'Medium', 'Hard', 'Very Hard'];
	const difficultyColors = ['#44ce1b', '#bbdb44', '#f7e379', '#f2a134', '#e51f1f'];
	const difficultyDescriptions = [
		'This project is very easy to get started with and is suitable for beginners.',
		'This project is relatively easy and may require some basic understanding of the technologies involved.',
		'This project has a moderate level of difficulty and may require some experience or research to complete.',
		'This project is challenging and may require significant experience or problem-solving skills to complete.',
		'This project is very difficult and may require advanced knowledge, creativity, and persistence to complete.'
	];

	const url_regex = /^(https?:\/\/)?(www\.)?github\.com\/[A-Za-z0-9_.-]+\/[A-Za-z0-9_.-]+\/?$/;

	function validateUrl(url: string): boolean {
		return url_regex.test(url);
	}

	async function handleUrlSubmit() {
		if (!isNotGithubDev) {
			if (validateUrl(projectUrl) === false) {
				continueEnabled = false;
				return;
			}

			activePage = 'loading';

			const match = projectUrl.match(/github\.com\/([^/]+)\/([^/]+)/);
			if (!match) return null;
			const ownerRepoObj = { owner: match[1], repo: match[2].replace(/\.git$/, '') };

			repoData = await octokit.repos.get(ownerRepoObj);

			data.projectName = repoData.data.name;
			data.description = repoData.data.description || '';
			data.url = projectUrl;
			data.programmingLanguage = repoData.data.language || '';
			data.notGithub = false;

			activePage = 'details';
			window.history.pushState({ page: 'details' }, '', '');
		} else {
			// For non GH, skip to details
			data.notGithub = true;
			activePage = 'details';
			window.history.pushState({ page: 'details' }, '', '');
		}
	}

	async function handleSubmit() {
		fetch(dev ? 'http://localhost:3000/projects/submit' : '/api/projects/submit', {
			method: 'POST',
			headers: {
				'Content-Type': 'application/json'
			},
			credentials: 'include',
			body: JSON.stringify(data)
		})
			.then((res) => {
				if (res.ok) {
					activePage = 'success';
					window.history.pushState({ page: 'success' }, '', '');
				} else {
					throw new Error('Failed to submit project');
				}
			})
			.catch((err) => {
				console.error('Error submitting project:', err);
				alert('There was an error submitting your project. Please try again later.');
			});
	}

	function goToFactorsPage() {
		activePage = 'factors';
		window.history.pushState({ page: 'factors' }, '', '');
	}

	function resetState() {
		activePage = 'url';
		continueEnabled = false;
		projectUrl = '';
		isNotGithubDev = false;
		repoData = null;
		data = {
			name: `${user?.firstName} ${user?.lastName}`,
			projectName: '',
			description: '',
			url: '',
			notGithub: false,
			programmingLanguage: '',
			improvementPotential: '',
			difficulty: 1,
			contactMethod: 'hackclub',
			contactInfo: ''
		};
	}

	function handleGoBack() {
		if (activePage === 'details') {
			activePage = 'url';
			window.history.pushState({ page: 'url' }, '', '');
		} else if (activePage === 'factors') {
			activePage = 'details';
			window.history.pushState({ page: 'details' }, '', '');
		} else if (activePage === 'success') {
			resetState();
		}
	}

	function handleNoRsvpContinue() {
		const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
		const slackIdRegex = /^@?[\w.-]+$/;

		if (emailRegex.test(noHackclubContact!)) {
			data.contactMethod = 'email';
			data.contactInfo = noHackclubContact!;
			noHackclubLogin = true;
		} else if (slackIdRegex.test(noHackclubContact!)) {
			data.contactMethod = 'slack';
			data.contactInfo = noHackclubContact!;
			noHackclubLogin = true;
		} else {
			alert('Please enter a valid email address or Slack ID.');
			return;
		}
	}

	onMount(() => {
		window.addEventListener('popstate', (event) => {
			handleGoBack();
		});

		fetch(dev ? 'http://localhost:3000/auth/check' : '/api/auth/check', {
			credentials: 'include'
		})
			.then((res) => res.json())
			.then((res) => {
				if (res.authenticated) {
					authenticated = true;
					user = res.user as User;
					data.name = `${user.firstName} ${user.lastName}`;
				} else {
					authenticated = false;
					user = null;
				}
			})
			.catch((err) => {
				console.error('Error checking authentication:', err);
			});
	});
</script>

<svelte:head>
	<link
		rel="stylesheet"
		href="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/devicon.min.css"
	/>
</svelte:head>

<title>Submit Project - Inherit</title>

{#if authenticated === true || noHackclubLogin === true}
	<div>
		<StylizedBox class="box">
			{#if activePage === 'url'}
				<img
					src="/mail_logo.svg"
					alt="Inherit Logo"
					style="width: 200px; height: auto; margin-bottom: 20px;"
				/>
				<div class="text">
					<h1>Submitting Your Project</h1>
					<p>Hey {user?.firstName}, thanks for your interest in submitting a project to Inherit!</p>
					<p>To submit your project please first enter your project url below:</p>

					<div class="input-outer">
						<input
							type="text"
							class="input-inner"
							placeholder="github.com/PokeMatPok/inheritYSWS"
							bind:value={projectUrl}
							oninput={() => {
								continueEnabled = validateUrl(projectUrl);
							}}
						/>
					</div>
					{#if projectUrl}
						<div class="url-input-indicator-wrapper">
							{#if !validateUrl(projectUrl) && !isNotGithubDev}
								<span class="url-input-indicator"
									>This does not look like a valid GitHub URL. <button
										onclick={() => (isNotGithubDev = true)}>Not developing on GitHub?</button
									></span
								>
							{:else}
								<span class="url-input-indicator">Looking good, just like you!</span>
							{/if}
						</div>
					{/if}

					<div class="buttons">
						<button
							class={`${!continueEnabled && !isNotGithubDev ? 'disabled' : ''}`}
							disabled={!continueEnabled && !isNotGithubDev}
							onclick={handleUrlSubmit}>Continue</button
						>
					</div>
				</div>
			{:else if activePage === 'loading'}
				<img
					src="/mail_logo.svg"
					alt="Inherit Logo"
					style="width: 200px; height: auto; margin-bottom: 20px;"
				/>
				<div class="text">
					<h2>Please wait a moment...</h2>
					<p>Getting everything ready...</p>
				</div>
			{:else if activePage === 'details'}
				<div class="text">
					<h1>Project Details</h1>
					<p>Almost there! Please provide some additional details about your project.</p>

					<div class="field-wrapper">
						<h3 class="field-descriptor">Title</h3>
						<div class="input-outer">
							<input class="input-inner" bind:value={data.projectName} />
						</div>
					</div>

					<div class="field-wrapper">
						<h3 class="field-descriptor">Project Description</h3>
						<div class="input-outer">
							<textarea class="input-inner multiline" bind:value={data.description}></textarea>
						</div>
					</div>

					<div class="field-wrapper">
						<h3 class="field-descriptor">Programming Language</h3>
						<div class="input-outer">
							<div class="language-icon-wrapper">
								<i
									class={`devicon-${data.programmingLanguage.trim().toLowerCase() || 'javascript'}-plain colored language-icon`}
								></i>
							</div>
							<input class="input-inner language" bind:value={data.programmingLanguage} />
						</div>
					</div>

					<div class="buttons">
						<button onclick={goToFactorsPage}>Continue</button>
					</div>
				</div>
			{:else if activePage === 'factors'}
				<div class="text">
					<h1>Project Factors</h1>
					<p>One last step! Tell us about how we should weight your project.</p>

					<div class="field-wrapper">
						<h3 class="field-descriptor">Improvement Potential</h3>
						<div class="input-outer">
							<textarea
								bind:value={data.improvementPotential}
								class="input-inner multiline"
								placeholder="Tell us what you would add in the future, which part needs improvement or how someone could continue your work."
							></textarea>
						</div>
					</div>

					<div class="field-wrapper">
						<h3 class="field-descriptor">Difficulty (Coffee-meter)</h3>
						<div class="coffee-selector">
							<button
								onclick={() => {
									if (data.difficulty && data.difficulty > 1) {
										data.difficulty = data.difficulty - 1;
									}
								}}
							>
								-
							</button>
							<div class="coffee-content">
								{#each Array(data.difficulty) as _, i (i)}
									<img
										src="/coffee.svg"
										alt="coffee cup"
										style="width: 30px; height: auto; margin: 0 2px;"
									/>
								{/each}
							</div>

							<button
								onclick={() => {
									if (data.difficulty && data.difficulty < 5) {
										data.difficulty = data.difficulty + 1;
									}
								}}
							>
								+
							</button>
						</div>
						<div class="difficulty-descriptor">
							<h4 style="color: {difficultyColors[data.difficulty - 1]};">
								{difficultyLabels[data!.difficulty - 1]}
							</h4>
							<p>{difficultyDescriptions[data.difficulty - 1]}</p>
						</div>
					</div>

					<div class="buttons">
						<button onclick={handleSubmit}>Submit</button>
					</div>
				</div>
			{:else if activePage === 'success'}
				<div class="text">
					<img
						src="/spaceship.svg"
						alt="Spaceship illustration"
						style="width: 300px; height: auto; margin-bottom: 20px;"
					/>
					{#if noHackclubLogin}
						<h1>One last step!</h1>
						<p>
							To keep in touch and let you know about your submission,
							<br />
							we need you to verify your contact information.
							<br />
							Please check your {data.contactMethod === 'email' ? 'email inbox' : 'Slack DMs'}
							for a message from us<br />and follow the instructions to complete your submission.
						</p>
					{:else}
						<h1>Success!</h1>
						<p>
							Your project has been submitted successfully.
							<br />
							We'll review it and get back to you soon!
						</p>
					{/if}
				</div>
			{/if}
		</StylizedBox>
	</div>
{:else if authenticated === false}
	<RsvpModal bind:this={rsvpModalRef} />

	<div>
		<StylizedBox class="box">
			<div class="text">
				<h1>Before You Submit</h1>
				<p>
					Please sign in to continue. Either RSVP with Hackclub <br /> or provide your contact information.
				</p>
				<button onclick={() => rsvpModalRef?.handleJoin('hackclub')} class="hc-link"
					><span class="hc-link-text"> Hackclub </span>
					<img
						src="https://assets.hackclub.com/icon-rounded.svg"
						alt="hackclub logo icon"
						class="hc-link-icon"
					/>
				</button>
				<br />
				or
				<div class="field-wrapper no-rsvp-signup">
					<div class="input-outer">
						<input
							type="text"
							class="input-inner"
							placeholder="E-mail or slack id"
							bind:value={noHackclubContact}
						/>

						<button onclick={handleNoRsvpContinue} class="email-slack-btn">
							<img class="submit-arrow" src="/arrow.svg" alt="Submit icon" />
						</button>
					</div>
				</div>
				<div class="no-rsvp-signup"></div>
			</div>
		</StylizedBox>
	</div>
{/if}

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

	.input-outer {
		background: repeating-linear-gradient(
				105deg,
				#8a5c37 0 6px,
				#614228 6px 12px,
				#69462c 12px 17px,
				#7c5433 17px 23px
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
		padding: 5px;

		display: flex;
		justify-content: center;
	}

	.input-outer:has(> .input-inner:focus) {
		background: repeating-linear-gradient(
				105deg,
				#6b4829 0 6px,
				#4a321a 6px 12px,
				#52351e 12px 17px,
				#5f3f24 17px 23px
			)
			padding-box;
	}

	.input-inner {
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
		padding: 10px;

		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		text-align: center;

		width: 90%;

		font-size: 1.2rem;
		color: var(--text-muted);
	}

	.multiline {
		height: 150px;
		text-align: start;
		padding-left: 2rem;
		font-family: sans-serif;
	}

	.url-input-indicator-wrapper {
		width: 100%;
		display: flex;
		justify-content: start;
		align-items: center;

		.url-input-indicator {
			font-size: 0.7rem;
			color: var(--text-muted);
			margin-left: 1rem;

			button {
				background: none;
				border: none;
				color: var(--text);
				font-family: var(--font);
				cursor: pointer;
				padding: 0;
				margin-left: 5px;
				font-size: 0.7rem;

				&:hover {
					text-decoration: underline;
				}
			}
		}
	}

	.field-wrapper {
		width: 100%;
		display: flex;
		flex-direction: column;
		align-items: start;
		gap: 0.05rem;
		margin-top: 1rem;

		.field-descriptor {
			font-size: 0.9rem;
			color: var(--text-muted);
			margin-left: 2rem;
			margin-bottom: 0;
		}

		.coffee-selector {
			display: flex;
			align-items: center;
			justify-content: space-between;
			width: 80%;
			margin-left: 10%;
			gap: 1rem;

			button {
				background-color: var(--brown-mid);
				border: solid 2px var(--brown-mid);
				color: var(--text);
				border-radius: 50%;
				width: 30px;
				height: 30px;
				display: flex;
				justify-content: center;
				align-items: center;
				font-size: 1.5rem;
				line-height: 0;
				cursor: pointer;
				box-shadow: 2px 2px 0px var(--brown-dark);

				&:hover {
					background-color: var(--brown-light);
					border-color: var(--brown-light);
					color: #1a0f05;
				}
			}
		}

		.difficulty-descriptor {
			width: 100%;
			display: flex;
			flex-direction: column;
			align-items: center;
			justify-content: center;

			h4 {
				margin: 0;
				font-size: 1.1rem;
				background-color: var(--bg-alt);
				border-radius: 20px;
				padding: 0.2rem 1rem;
				display: inline-block;
			}

			p {
				margin: 0.2rem 0 0 0;
				font-size: 0.9rem;
				color: var(--text-muted);
				max-width: 400px;
			}
		}

		.input-outer {
			width: 90%;
			margin-top: 0;

			.language-icon-wrapper {
				margin-left: 2rem;
				display: flex;
				align-items: center;
				justify-content: center;

				.language-icon {
					font-size: 1.5rem;
					padding: 1rem;
					padding-right: 1.3rem;
					border-radius: 50%;
					color: var(--brown-mid);
					background-color: var(--brown-dark);
					pointer-events: none;
					width: 30%;
				}
			}
		}
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

			&:not(.disabled):hover {
				background-color: var(--brown-light);
				border-color: var(--brown-light);
				color: #1a0f05;
			}

			&.disabled {
				cursor: not-allowed;
			}
		}
	}

	.disabled {
		opacity: 0.6;
		cursor: not-allowed;
	}

	.hc-link {
		border: none;
		padding: 15px;
		margin: 5px;
		border-radius: 20px;
		display: inline-flex;
		align-items: center;
		font-family: 'Phantom Sans', sans-serif;
		font-size: 20px;
		cursor: pointer;
		background-color: #ec3750;
		color: white;
	}

	.hc-link-icon {
		width: 20px;
		height: 20px;
		margin-left: 8px;
	}

	.no-rsvp-signup {
		margin-top: 2rem;
		font-size: 0.9rem;
		display: flex;
		align-items: center;
		justify-content: center;
		width: 100%;

		.input-outer {
			width: 90%;
			margin-right: 1rem;

			.email-slack-btn {
				border: none;
				padding: 15px;
				margin: 5px;
				border-radius: 20px;
				display: inline-flex;
				align-items: center;
				font-family: 'Phantom Sans', sans-serif;
				font-size: 20px;
				cursor: pointer;
				background-color: transparent;
				color: #333;

				.submit-arrow {
					width: 30px;
					height: 30px;
				}
			}
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

	@media (max-width: 600px) {
		.input-inner {
			font-size: 1rem;
		}

		.buttons {
			flex-direction: column;
			gap: 1rem;
		}

		.url-input-indicator {
			font-size: 1rem;

			button {
				font-size: 1rem;
			}
		}
	}
</style>
