<script lang="ts">
	import type { Snippet } from 'svelte';

	let {
		class: className = '',
		element = $bindable(),
		children
	}: {
		class?: string;
		element?: HTMLDivElement;
		children?: Snippet;
	} = $props();
</script>

<div class="content {className}" bind:this={element}>
	<div class="content-inner">
		{@render children?.()}
	</div>
</div>

<style>
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
</style>
