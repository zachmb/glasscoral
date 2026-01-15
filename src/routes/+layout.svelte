<script lang="ts">
	import './layout.css';
	import favicon from '$lib/assets/favicon.svg';
	import { page } from '$app/stores';
	import { fly } from 'svelte/transition';
	import { cubicOut } from 'svelte/easing';

	let { children } = $props();
</script>

<svelte:head>
	<link rel="icon" href={favicon} />
</svelte:head>

<!-- Liquid Glass Background -->
<div class="fixed inset-0 z-[-1] overflow-hidden pointer-events-none">
	<!-- Obsidian Base -->
	<div class="absolute inset-0 bg-obsidian"></div>
	
	<!-- Coral Mesh Blob -->
	<div class="absolute top-[-20%] left-[-10%] w-[70vw] h-[70vw] rounded-full bg-coral opacity-20 blur-[120px] animate-pulse-slow"></div>
	
	<!-- Violet Mesh Blob -->
	<div class="absolute bottom-[-20%] right-[-10%] w-[70vw] h-[70vw] rounded-full bg-violet opacity-20 blur-[120px] animate-pulse-slow delay-1000"></div>
	
	<!-- Noise/Texture Overlay (Optional, for extra glass feel) -->
	<div class="absolute inset-0 opacity-[0.03]" style="background-image: url('data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSI0IiBoZWlnaHQ9IjQiPgo8cmVjdCB3aWR0aD0iNCIgaGVpZ2h0PSI0IiBmaWxsPSIjZmZmIi8+CjxyZWN0IHdpZHRoPSIxIiBoZWlnaHQ9IjEiIGZpbGw9IiMwMDAiLz4KPC9zdmc+');"></div>
</div>

<!-- Page Content with Transition -->
{#key $page.url.pathname}
	<main 
		class="min-h-screen w-full relative"
		in:fly={{ y: 20, duration: 600, delay: 200, easing: cubicOut }}
		out:fly={{ y: -20, duration: 400, easing: cubicOut }}
	>
		{@render children()}
	</main>
{/key}

<style>
	/* Custom slow pulse animation for the blobs */
	@keyframes pulse-slow {
		0%, 100% { transform: scale(1) translate(0, 0); opacity: 0.2; }
		50% { transform: scale(1.1) translate(2%, 2%); opacity: 0.15; }
	}
	.animate-pulse-slow {
		animation: pulse-slow 8s ease-in-out infinite;
	}
</style>
