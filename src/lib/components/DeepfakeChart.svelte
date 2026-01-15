<script lang="ts">
	import { onMount } from 'svelte';
	import { tweened } from 'svelte/motion';
	import { cubicOut } from 'svelte/easing';
	import { fade, fly } from 'svelte/transition';

	let visible = false;

	const progress = tweened(0, {
		duration: 1500,
		easing: cubicOut
	});

	onMount(() => {
		const observer = new IntersectionObserver(
			(entries) => {
				if (entries[0].isIntersecting) {
					visible = true;
					progress.set(1);
				}
			},
			{ threshold: 0.5 }
		);

		const chart = document.getElementById('deepfake-chart');
		if (chart) observer.observe(chart);

		return () => observer.disconnect();
	});
</script>

<div
	id="deepfake-chart"
	class="group relative flex h-[400px] w-full items-end justify-center overflow-hidden rounded-2xl border border-white/5 bg-white/[0.02] p-8 backdrop-blur-sm"
>
	<!-- Grid Lines -->
	<div class="absolute inset-0 opacity-10">
		{#each Array(5) as _, i}
			<div class="absolute h-px w-full bg-white top-[{i * 25}%]"></div>
		{/each}
	</div>

	<!-- Chart Container -->
	<div class="relative flex h-full w-full items-end px-12 pb-8">
		<!-- 2019 Point -->
		<div class="absolute bottom-10 left-10 flex flex-col items-center">
			<div class="mb-2 h-3 w-3 rounded-full bg-white/50"></div>
			<span class="font-mono text-sm text-white/40">2019</span>
			<span class="absolute -top-8 -mt-8 text-lg font-bold text-white">8k</span>
		</div>

		<!-- 2025 Point (Animated) -->
		<div
			class="absolute right-10 flex flex-col items-center"
			style="bottom: {$progress * 60 + 10}%"
		>
			<div class="mb-2 h-4 w-4 rounded-full bg-[#FF6B6B] shadow-[0_0_20px_#FF6B6B]"></div>
			<span class="absolute top-6 font-mono text-sm text-white/40">2025</span>
			<span class="absolute -top-10 -mt-10 text-3xl font-bold whitespace-nowrap text-[#FF6B6B]"
				>550k</span
			>
		</div>

		<!-- The Line (SVG) -->
		<svg
			class="pointer-events-none absolute inset-0 h-full w-full overflow-visible"
			preserveAspectRatio="none"
		>
			<defs>
				<linearGradient id="gradient" x1="0" y1="0" x2="0" y2="1">
					<stop offset="0%" stop-color="#FF6B6B" stop-opacity="0.3" />
					<stop offset="100%" stop-color="#FF6B6B" stop-opacity="0" />
				</linearGradient>
			</defs>

			<!-- Area -->
			<path
				d="M 40,360 Q 300,360 700,{$progress * -225 + 360} L 700,360 Z"
				fill="url(#gradient)"
				class="transition-all duration-300"
			/>

			<!-- Line -->
			<path
				d="M 40,360 Q 300,360 700,{$progress * -225 + 360}"
				fill="none"
				stroke="#FF6B6B"
				stroke-width="3"
				stroke-linecap="round"
				class="drop-shadow-[0_0_10px_rgba(255,107,107,0.5)]"
			/>
		</svg>

		<!-- Annotation -->
		{#if visible}
			<div
				in:fly={{ y: 20, duration: 800, delay: 1000 }}
				class="absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 rounded-lg border border-[#FF6B6B]/20 bg-[#FF6B6B]/10 px-4 py-2 text-[#FF6B6B] backdrop-blur-md"
			>
				<span class="font-bold">6,600% Increase</span>
			</div>
		{/if}
	</div>
</div>
