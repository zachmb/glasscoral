<script lang="ts">
	import './layout.css';

	let { children } = $props();
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

<!-- Page Content - Instant Navigation -->
<main class="relative min-h-screen w-full">
	{@render children()}
</main>

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
