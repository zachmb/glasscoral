<script lang="ts">
	import { spring } from 'svelte/motion';

	let { title, description, class: className = '', children } = $props();

	let hover = $state(false);

	const scale = spring(1, {
		stiffness: 0.1,
		damping: 0.25
	});

	function handleMouseEnter() {
		hover = true;
		scale.set(1.02);
	}

	function handleMouseLeave() {
		hover = false;
		scale.set(1);
	}
</script>

<div
	class="group relative cursor-default overflow-hidden rounded-3xl border border-white/10 bg-white/[0.02] p-8 backdrop-blur-xl transition-colors duration-500 hover:bg-white/[0.04] {className}"
	style="transform: scale({$scale})"
	onmouseenter={handleMouseEnter}
	onmouseleave={handleMouseLeave}
	role="article"
>
	<!-- Inner Highlight/Glow -->
	<div
		class="pointer-events-none absolute inset-0 opacity-0 transition-opacity duration-700 group-hover:opacity-100"
		style="background: radial-gradient(800px circle at var(--mouse-x) var(--mouse-y), rgba(255,255,255,0.06), transparent 40%)"
	></div>

	<div class="relative z-10 flex h-full flex-col justify-between">
		<!-- Icon/Slot Area -->
		<div class="mb-6 text-white/80">
			{@render children?.()}
		</div>

		<!-- Content -->
		<div>
			<h3
				class="group-hover:text-coral-300 mb-2 text-2xl font-bold tracking-tight text-white transition-colors duration-300"
			>
				{title}
			</h3>
			<p
				class="text-sm leading-relaxed text-white/50 transition-colors duration-300 group-hover:text-white/70"
			>
				{description}
			</p>
		</div>
	</div>

	<!-- Glass Border Effect -->
	<div
		class="pointer-events-none absolute inset-0 rounded-3xl border border-white/10 mix-blend-overlay"
	></div>
</div>
