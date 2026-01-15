<script>
	import Button from '$lib/components/Button.svelte';
</script>

<div class="min-h-screen bg-[#020408] text-white selection:bg-orange-500/30">
	<div class="relative flex min-h-screen flex-col items-center justify-center p-6 text-center">
		<!-- Background Gradient -->
		<div
			class="pointer-events-none absolute inset-0 bg-gradient-to-br from-orange-500/10 via-blue-500/5 to-transparent"
		></div>

		<h1 class="mb-4 font-display text-7xl font-bold tracking-tighter md:text-9xl">
			<span class="bg-gradient-to-r from-orange-400 to-blue-500 bg-clip-text text-transparent"
				>Kazwire</span
			>
		</h1>
		<p class="mb-8 max-w-2xl text-xl text-white/60">
			Next-gen web gaming platform reaching 4M+ users with 120M+ page views.
		</p>
		<Button variant="ghost">Visit Platform</Button>
	</div>
</div>
