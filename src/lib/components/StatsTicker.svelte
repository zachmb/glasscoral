<script lang="ts">
	import { onMount } from 'svelte';
	import { fly } from 'svelte/transition';

	// Stats Data
	const stats = [
		{ value: '$1 Million', label: 'ARR Reached' },
		{ value: '5x', label: 'Exits this Year' },
		{ value: '99%', label: 'Industry Fail Rate' },
		{ value: '1M+', label: 'Active Users' }
	];

	let visible = false;

	onMount(() => {
		visible = true;
	});
</script>

<div class="w-full border-y border-white/5 bg-white/5 backdrop-blur-md">
	<div
		class="mx-auto flex max-w-7xl flex-col items-center justify-between gap-8 py-32 md:flex-row md:px-6"
	>
		<div class="max-w-md space-y-4 px-6 text-center md:text-left">
			<h3 class="font-display text-2xl font-bold text-white">DEPTH & SCALE</h3>
			<p class="text-white/60">
				99% of companies never get to $1Million ARR, but we did it 5 times this year alone. A tech
				company mainly focused on building, acquiring and managing cool projects.
			</p>
		</div>

		<div class="grid grid-cols-2 gap-8 px-6 md:gap-16">
			{#each stats as stat, i}
				{#if visible}
					<div
						in:fly={{ y: 20, delay: i * 100, duration: 800 }}
						class="flex flex-col items-center md:items-start"
					>
						<span class="font-display text-3xl font-bold text-white md:text-4xl">{stat.value}</span>
						<span class="text-sm font-light tracking-wide text-white/50 uppercase"
							>{stat.label}</span
						>
					</div>
				{/if}
			{/each}
		</div>
	</div>
</div>
