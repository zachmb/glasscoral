<script lang="ts">
	import { onMount } from 'svelte';
	import { Motion, useAnimation } from 'svelte-motion';

	// Drifters configuration
	const drifters = [
		{ size: 400, x: -100, y: 100, delay: 0, duration: 25, color: '#FF6B6B' }, // Coral
		{ size: 500, x: 800, y: -200, delay: 2, duration: 30, color: '#4ECDC4' }, // Tealish
		{ size: 300, x: 200, y: 600, delay: 5, duration: 35, color: '#FF6B6B' }, // Coral
		{ size: 600, x: -300, y: -100, delay: 1, duration: 40, color: '#2C3E50' } // Dark Blue
	];
</script>

<div class="pointer-events-none fixed inset-0 z-0 overflow-hidden">
	{#each drifters as drifter, i}
		<div
			class="absolute rounded-full opacity-20 blur-[100px]"
			style="
        width: {drifter.size}px;
        height: {drifter.size}px;
        background-color: {drifter.color};
        left: {drifter.x}px;
        top: {drifter.y}px;
        animation: float-{i} {drifter.duration}s infinite ease-in-out alternate;
      "
		></div>
	{/each}
</div>

<style>
	@keyframes float-0 {
		0% {
			transform: translate(0, 0) rotate(0deg);
		}
		100% {
			transform: translate(100px, 150px) rotate(20deg);
		}
	}
	@keyframes float-1 {
		0% {
			transform: translate(0, 0) scale(1);
		}
		100% {
			transform: translate(-150px, 100px) scale(1.1);
		}
	}
	@keyframes float-2 {
		0% {
			transform: translate(0, 0) rotate(0);
		}
		100% {
			transform: translate(100px, -100px) rotate(-15deg);
		}
	}
	@keyframes float-3 {
		0% {
			transform: translate(0, 0);
		}
		100% {
			transform: translate(-50px, 200px);
		}
	}
</style>
