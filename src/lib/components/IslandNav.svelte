<script lang="ts">
	import CoralIcon from '$lib/components/CoralIcon.svelte';
	import { onMount } from 'svelte';

	let isScrolled = false;

	onMount(() => {
		const handleScroll = () => {
			isScrolled = window.scrollY > 50;
		};
		window.addEventListener('scroll', handleScroll);
		return () => window.removeEventListener('scroll', handleScroll);
	});
</script>

<nav
	class="fixed top-8 left-1/2 z-50 -translate-x-1/2 transition-all duration-500 ease-out {isScrolled
		? 'w-[90vw] md:w-[600px]'
		: 'w-[95vw] md:w-[650px]'}"
>
	<div class="group relative">
		<!-- Glass Background -->
		<div
			class="absolute inset-0 rounded-full border border-white/10 bg-white/[0.03] shadow-[0_8px_32px_0_rgba(0,0,0,0.36)] backdrop-blur-xl transition-all duration-300 group-hover:scale-[1.02] group-hover:bg-white/[0.05]"
		></div>

		<!-- Content -->
		<div class="relative flex items-center justify-between px-6 py-4 md:px-8">
			<!-- Logo -->
			<a
				href="/"
				class="hover:text-coral-400 flex items-center gap-3 text-lg font-bold tracking-tight text-white transition-colors"
			>
				<CoralIcon size={32} class="text-coral-400" />
				<span class="hidden sm:inline">GLASSCORAL</span>
			</a>

			<!-- Links -->
			<div class="flex items-center space-x-1 md:space-x-6">
				<a
					href="#studios"
					class="text-xs text-white/60 transition-colors hover:text-white md:text-sm">Reef</a
				>
				<a
					href="#methodology"
					class="hidden text-sm text-white/60 transition-colors hover:text-white sm:inline-block"
					>Methodology</a
				>
				<a
					href="#methodology"
					class="text-xs text-white/60 transition-colors hover:text-white md:hidden">Process</a
				>
			</div>

			<!-- CTA -->
			<a
				href="/login"
				class="rounded-full border border-white/5 bg-white/10 px-4 py-2 text-xs font-medium text-white transition-all hover:border-white/20 hover:bg-white/20 md:text-sm"
			>
				Join Studio
			</a>
		</div>
	</div>
</nav>

<style>
	:global(.text-coral-400) {
		color: #ff6b6b;
	}
</style>
