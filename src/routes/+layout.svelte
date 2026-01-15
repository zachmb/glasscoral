<script lang="ts">
	import './layout.css';
	import { page } from '$app/stores';
	import { fly } from 'svelte/transition';
	import { cubicOut } from 'svelte/easing';
	import { afterNavigate, beforeNavigate } from '$app/navigation';

	let { children } = $props();

	// Route Order Definition
	const routes = ['/', '/portfolio'];

	let direction = $state(1);
	let transitionDuration = 600;

	beforeNavigate(({ to, from }) => {
		const fromPath = from?.url.pathname ?? '/';
		const toPath = to?.url.pathname ?? '/';

		const fromIndex = routes.indexOf(fromPath);
		const toIndex = routes.indexOf(toPath);

		// Default to forward (1) if moving down the list or to unknown
		if (fromIndex !== -1 && toIndex !== -1) {
			direction = toIndex > fromIndex ? 1 : -1;
		} else {
			// Fallback for non-listed routes
			direction = 1;
		}
	});

	// Helper for animation params
	function getTransitionParams() {
		return {
			x: direction * 100, // Distance to slide
			duration: transitionDuration,
			easing: cubicOut,
			opacity: 0 // Fade as well
		};
	}
</script>

<!-- Liquid Glass Background -->
<div class="pointer-events-none fixed inset-0 z-[-1] overflow-hidden">
	<!-- Obsidian Base -->
	<div class="absolute inset-0 bg-obsidian"></div>

	<!-- Coral Mesh Blob -->
	<div
		class="animate-pulse-slow absolute top-[-20%] left-[-10%] h-[70vw] w-[70vw] rounded-full bg-coral opacity-20 blur-[120px]"
	></div>

	<!-- Violet Mesh Blob -->
	<div
		class="animate-pulse-slow absolute right-[-10%] bottom-[-20%] h-[70vw] w-[70vw] rounded-full bg-violet opacity-20 blur-[120px] delay-1000"
	></div>

	<!-- Noise/Texture Overlay (Optional, for extra glass feel) -->
	<div
		class="absolute inset-0 opacity-[0.03]"
		style="background-image: url('data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSI0IiBoZWlnaHQ9IjQiPgo8cmVjdCB3aWR0aD0iNCIgaGVpZ2h0PSI0IiBmaWxsPSIjZmZmIi8+CjxyZWN0IHdpZHRoPSIxIiBoZWlnaHQ9IjEiIGZpbGw9IiMwMDAiLz4KPC9zdmc+');"
	></div>
</div>

<!-- Page Content with Transition -->
<div class="grid min-h-screen w-full grid-cols-1 grid-rows-1 overflow-x-hidden">
	{#key $page.url.pathname}
		<main
			class="col-start-1 row-start-1 min-h-screen w-full"
			in:fly={{
				x: direction * 300,
				duration: transitionDuration,
				easing: cubicOut,
				opacity: 1
			}}
			out:fly={{ x: direction * -300, duration: transitionDuration, easing: cubicOut, opacity: 1 }}
		>
			{@render children()}
		</main>
	{/key}
</div>

<style>
	/* Custom slow pulse animation for the blobs */
	@keyframes pulse-slow {
		0%,
		100% {
			transform: scale(1) translate(0, 0);
			opacity: 0.2;
		}
		50% {
			transform: scale(1.1) translate(2%, 2%);
			opacity: 0.15;
		}
	}
	.animate-pulse-slow {
		animation: pulse-slow 8s ease-in-out infinite;
	}
</style>
