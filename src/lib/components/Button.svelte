<script lang="ts">
	import { spring } from 'svelte/motion';

	interface Props {
		children?: import('svelte').Snippet;
		text?: string;
		variant?: 'coral' | 'ghost';
		href?: string;
		class?: string;
		onclick?: () => void;
	}

	let { children, text, variant = 'coral', href, class: className = '', onclick }: Props = $props();

	// Spring physics for click scaling
	// Note: Approx map of "stiffness 100, damping 20" (likely from React Spring) to Svelte
	// increased stiffness and damping for a snappy but stable feel.
	const scale = spring(1, { stiffness: 0.15, damping: 0.3 });

	function handleMouseDown() {
		scale.set(0.98);
	}

	function handleMouseUp() {
		scale.set(1);
	}

	function handleMouseEnter() {
		// Hover effect handled by CSS, but could go here
	}

	const baseClasses =
		'relative px-6 py-3 rounded-xl font-medium tracking-tight transition-all duration-300 flex items-center justify-center isolate overflow-hidden cursor-pointer select-none';

	const variants = {
		coral:
			'bg-[#FF6B6B] text-white shadow-[0_0_20px_rgba(255,107,107,0.4)] hover:shadow-[0_0_30px_rgba(255,107,107,0.6)] border border-white/10 hover:bg-[#ff8585]',
		ghost: 'bg-white/5 backdrop-blur-md border border-white/10 text-white/90 hover:bg-white/10'
	};
</script>

<!-- Shimmer Effect Helper -->
{#snippet shimmer()}
	<div
		class="pointer-events-none absolute inset-0 z-[-1] -translate-x-full bg-gradient-to-r from-transparent via-white/10 to-transparent group-hover:animate-[shimmer_1.5s_infinite]"
	></div>
{/snippet}

{#if href}
	<a
		{href}
		class="{baseClasses} {variants[variant]} group {className}"
		style="transform: scale({$scale})"
		onmousedown={handleMouseDown}
		onmouseup={handleMouseUp}
		onmouseleave={handleMouseUp}
		{onclick}
	>
		{@render shimmer()}
		{#if children}
			{@render children()}
		{:else}
			{text}
		{/if}
	</a>
{:else}
	<button
		class="{baseClasses} {variants[variant]} group {className}"
		style="transform: scale({$scale})"
		onmousedown={handleMouseDown}
		onmouseup={handleMouseUp}
		onmouseleave={handleMouseUp}
		{onclick}
	>
		{@render shimmer()}
		{#if children}
			{@render children()}
		{:else}
			{text}
		{/if}
	</button>
{/if}
