<script lang="ts">
	import { createEventDispatcher } from 'svelte';
	import { crossfade } from 'svelte/transition';
	import { cubicInOut } from 'svelte/easing';

	export let options = [
		'Conversations',
		'Documents',
		'Images & Video',
		'Social Media',
		'Everyday Use'
	];

	export let activeTab = options[0];

	const dispatch = createEventDispatcher();

	const [send, receive] = crossfade({
		duration: 300,
		easing: cubicInOut
	});

	function selectTab(option: string) {
		activeTab = option;
		dispatch('change', option);
	}
</script>

<div
	class="no-scrollbar flex max-w-full items-center space-x-2 overflow-x-auto rounded-full border border-white/10 bg-white/[0.03] p-1.5 backdrop-blur-lg"
>
	{#each options as option}
		<button
			on:click={() => selectTab(option)}
			class="relative rounded-full px-6 py-2.5 text-sm font-medium whitespace-nowrap transition-colors duration-300 {activeTab ===
			option
				? 'text-white'
				: 'text-white/50 hover:text-white/80'}"
		>
			{#if activeTab === option}
				<div
					in:receive={{ key: 'active-indicator' }}
					out:send={{ key: 'active-indicator' }}
					class="absolute inset-0 rounded-full border border-white/10 bg-white/10 shadow-[0_0_15px_rgba(255,107,107,0.3)]"
				></div>
			{/if}
			<span class="relative z-10">{option}</span>
		</button>
	{/each}
</div>

<style>
	.no-scrollbar::-webkit-scrollbar {
		display: none;
	}
	.no-scrollbar {
		-ms-overflow-style: none;
		scrollbar-width: none;
	}
</style>
