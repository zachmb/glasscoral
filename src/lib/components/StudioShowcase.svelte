<script lang="ts">
	import { ArrowRight, MoveRight } from 'lucide-svelte';

	// Projects Data
	const projects = [
		{
			year: '2024',
			name: 'Kazwire',
			description: 'The next evolution in digital connectivity and high-speed data transmission.',
			link: '#'
		},
		{
			year: '2024',
			name: 'Loopless',
			description: 'Seamless financial infrastructure for the modern creator economy.',
			link: '#'
		},
		{
			year: '2024',
			name: 'Deo.so',
			description:
				'A calendar that thinks for you. AI-driven scheduling for high-performance teams.',
			link: '#'
		},
		{
			year: '2023',
			name: 'YouTube Channel Network',
			description: 'A massive network of automated content channels generating millions of views.',
			link: '#'
		},
		{
			year: '2023',
			name: 'PredictAdmit.com',
			description: 'AI-powered college admission predictions with 95% accuracy.',
			link: '#'
		}
	];
</script>

<div class="w-full space-y-12">
	<div class="flex items-center justify-between border-b border-white/10 pb-4">
		<h2 class="font-display text-sm tracking-widest text-white/50 uppercase">
			/// PUBLIC PORTFOLIO
		</h2>
		<span class="font-mono text-sm text-white/30">2023 — 2024</span>
	</div>

	<div class="grid gap-6 sm:grid-cols-2 lg:grid-cols-3">
		{#each projects as project}
			<div
				class="group relative flex flex-col justify-between overflow-hidden rounded-2xl border border-white/5 bg-white/5 p-8 backdrop-blur-sm transition-all duration-300 hover:border-white/20 hover:bg-white/10"
			>
				<div class="mb-8 space-y-4">
					<span class="text-coral-400 font-mono text-xs">{project.year}</span>
					<h3 class="font-display text-2xl font-bold text-white">{project.name}</h3>
					<p class="text-sm leading-relaxed text-white/60">
						{project.description}
					</p>
				</div>

				<a
					href={project.link}
					class="flex w-fit items-center gap-2 text-xs font-bold tracking-wider text-white uppercase transition-opacity hover:opacity-80"
				>
					Visit <MoveRight size={14} class="transition-transform group-hover:translate-x-1" />
				</a>

				<!-- Hover Gradient -->
				<div
					class="from-coral-500/10 absolute inset-0 z-[-1] bg-gradient-to-tr to-transparent opacity-0 transition-opacity duration-500 group-hover:opacity-100"
				></div>
			</div>
		{/each}
	</div>
</div>
