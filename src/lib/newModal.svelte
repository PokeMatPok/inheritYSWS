<script lang="ts">
	let isOpen = true;
	let activePage: 'lang_sel' | 'difficulty_sel' | 'loading' | 'result' = 'lang_sel';

	function openModal() {
		isOpen = true;
	}

	function closeModal() {
		isOpen = false;
	}

	function handleKeyPress(event: KeyboardEvent) {
		if (event.key === 'Escape') {
			closeModal();
		}
	}
</script>

{#if isOpen}
	<div
		class="modal-overlay"
		on:click={closeModal}
		on:keydown={handleKeyPress}
		role="presentation"
		tabindex="-1"
	>
		<!-- svelte-ignore a11y_click_events_have_key_events -->
		<div
			class="modal-content"
			on:click|stopPropagation
			role="dialog"
			aria-modal="true"
			aria-labelledby="modal-title"
			tabindex="0"
		>
			<!-- different pages -->
			{#if activePage === 'lang_sel'}
				<h2 id="modal-title">Select your language:</h2>
				<div>
					<div>
						{#each ['Javascript', 'Python', 'Rust', 'Go'] as name (name)}
							<button
								on:click={() => (activePage = 'difficulty_sel')}
								aria-label={`Select ${name} as your programming language`}
							>
								<i class="devicon-{name.toLowerCase()}-plain colored"></i>
								{name}
							</button>
						{/each}
					</div>
					<div></div>
				</div>
			{:else if activePage === 'difficulty_sel'}
                <h2 id="modal-title">Select your difficulty:</h2>

                {#each ['Easy', 'Medium', 'Hard'] as difficulty (difficulty)}
                    <button
                        on:click={() => (activePage = 'loading')}
                        aria-label={`Select ${difficulty} as your difficulty level`}
                    >
                        {difficulty}
                    </button>
                {/each}
            {:else if activePage === 'loading'}
                <h2 id="modal-title">Loading...</h2>
                <p>Please wait while we generate your code snippet.</p>
            {:else if activePage === 'result'}
                <h2 id="modal-title">Your Code Snippet</h2>
                <pre><code>// Your generated code snippet will appear here.</code></pre>
            {/if}
		</div>
	</div>
{/if}

<button on:click={openModal}>Open Modal</button>

<style>
	.modal-overlay {
		position: fixed;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
		background-color: rgba(0, 0, 0, 0.5);
		display: flex;
		justify-content: center;
		align-items: center;
		z-index: 9999;
	}

	.modal-content {
		background-color: white;
		padding: 2rem;
		border-radius: 8px;
		box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
	}

	button {
		padding: 0.5rem 1rem;
		background-color: #007bff;
		color: white;
		border: none;
		border-radius: 4px;
		cursor: pointer;
	}

	button:hover {
		background-color: #0056b3;
	}
</style>
