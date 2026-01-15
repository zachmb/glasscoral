<script>
	import Button from '$lib/components/Button.svelte';
</script>

<div class="min-h-screen bg-[#020408] text-white selection:bg-purple-500/30">
	<div class="relative flex min-h-screen flex-col items-center justify-center p-6 text-center">
		<!-- Background Gradient -->
		<div
			class="pointer-events-none absolute inset-0 bg-gradient-to-br from-purple-500/10 via-white/5 to-transparent"
		></div>

		<h1 class="mb-4 font-display text-7xl font-bold tracking-tighter md:text-9xl">
			<span
				class="bg-gradient-to-r from-purple-400 via-white to-purple-400 bg-clip-text text-transparent"
				>Deo.so</span
			>
		</h1>
		<p class="mb-8 max-w-2xl text-xl text-white/60">
			The AI Operating System for Social Media Management.
		</p>
		<Button variant="ghost">Learn More</Button>
	</div>
</div>
