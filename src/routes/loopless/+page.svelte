<script>
	import Button from '$lib/components/Button.svelte';
</script>

<div class="min-h-screen bg-[#020408] text-white selection:bg-cyan-500/30">
	<div class="relative flex min-h-screen flex-col items-center justify-center p-6 text-center">
		<!-- Background Gradient -->
		<div
			class="pointer-events-none absolute inset-0 bg-gradient-to-br from-cyan-500/10 via-blue-900/10 to-transparent"
		></div>

		<h1 class="mb-4 font-display text-7xl font-bold tracking-tighter md:text-9xl">
			<span class="bg-gradient-to-r from-cyan-400 to-blue-800 bg-clip-text text-transparent"
				>Loopless</span
			>
		</h1>
		<p class="mb-8 max-w-2xl text-xl text-white/60">
			CBT-based screen time reduction. 10M+ views helping users reclaim their attention.
		</p>
		<Button variant="ghost">Get the App</Button>
	</div>
</div>
