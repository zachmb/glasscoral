<script lang="ts">
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
		? 'w-[600px]'
		: 'w-[650px]'}"
>
	<div class="group relative">
		<!-- Glass Background -->
		<div
			class="absolute inset-0 rounded-full border border-white/10 bg-white/[0.03] shadow-[0_8px_32px_0_rgba(0,0,0,0.36)] backdrop-blur-xl transition-all duration-300 group-hover:scale-[1.02] group-hover:bg-white/[0.05]"
		></div>

		<!-- Content -->
		<div class="relative flex items-center justify-between px-8 py-4">
			<!-- Logo -->
			<a
				href="/"
				class="hover:text-coral-400 flex items-center gap-3 text-lg font-bold tracking-tight text-white transition-colors"
			>
				<img src="/logo.svg" alt="GlassCoral Logo" class="h-8 w-8" />
				GlassCoral
			</a>

			<!-- Links -->
			<div class="hidden items-center space-x-6 md:flex">
				<a href="#features" class="text-sm text-white/60 transition-colors hover:text-white"
					>Features</a
				>
				<a href="#how-it-works" class="text-sm text-white/60 transition-colors hover:text-white"
					>How it Works</a
				>
			</div>

			<!-- CTA -->
			<a
				href="/login"
				class="rounded-full border border-white/5 bg-white/10 px-4 py-2 text-sm font-medium text-white transition-all hover:border-white/20 hover:bg-white/20"
			>
				Login
			</a>
		</div>
	</div>
</nav>

<style>
	:global(.text-coral-400) {
		color: #ff6b6b;
	}
</style>
