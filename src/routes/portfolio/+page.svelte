<script lang="ts">
	import { onMount } from 'svelte';
	import { fly } from 'svelte/transition';
	import { cubicOut } from 'svelte/easing';
	import CoralDrifters from '$lib/components/CoralDrifters.svelte';
	import IslandNav from '$lib/components/IslandNav.svelte';
	import CoralField from '$lib/components/CoralField.svelte';
	import CoralBranch from '$lib/components/CoralBranch.svelte';
	import Button from '$lib/components/Button.svelte';

	// Intersection Observer Action
	function viewport(element: HTMLElement, params: { once?: boolean } = {}) {
		let observer: IntersectionObserver;

		function handleIntersect(entries: IntersectionObserverEntry[]) {
			entries.forEach((entry) => {
				if (entry.isIntersecting) {
					element.dispatchEvent(new CustomEvent('viewportenter'));
					if (params.once) observer.unobserve(element);
				}
			});
		}

		onMount(() => {
			observer = new IntersectionObserver(handleIntersect, { threshold: 0.1 });
			observer.observe(element);
			return () => observer.disconnect();
		});
	}

	let visible = $state(false);

	const projects = [
		{
			name: 'Kazwire',
			tagline:
				'The only web-gaming site with a sustained UGC team -- that operates at sub $0.50 CPM.',
			description: 'Over 3,800,000 users. Efficient load-balancing and performance optimized.',
			stats: ['Sub $0.50 CPM UGC team', 'Over 120,000,000 page views'],
			image: '/images/kazwire.png',
			link: 'https://kazwire.com',
			color: 'text-cyan-400'
		},
		{
			name: 'LoopLess',
			tagline: 'A first-of-its-kind science based screen time reduction app that actually works.',
			description:
				'4.7 stars and viral social media success. Cognitive-Behavioral Therapy Based. Fully Swift stack.',
			stats: ['5,000,000+ UGC views', 'Team from Fermilab, Apple, OpenAI'],
			image: '/images/loopless.png',
			link: '#',
			color: 'text-emerald-400'
		},
		{
			name: 'Deo.so',
			tagline: 'AI powered social media marketing platform. Enhance, not replace, human creatives.',
			description:
				'Hire, manage, and scale your marketing team. State-of-the-art AI video generation to boost views.',
			stats: ['200k+ LinkedIn searches', 'Invites from billion dollar company execs'],
			image: '/images/deo.png',
			link: 'https://deo.so',
			color: 'text-purple-400'
		},
		{
			name: 'YouTube Channels',
			tagline:
				'We operate a network of YouTube channels focused on gaming content, education, and technology.',
			description:
				'Achieving massive reach and engagement. Brand partnerships e.g. with now.gg. Features by massive streamers.',
			stats: ['23,000,000+ views', '20,000+ subscribers'],
			image: '/images/youtube.png',
			link: '#',
			color: 'text-red-400'
		},
		{
			name: 'Stealth Mode',
			tagline:
				'We have more businesses launched and in development that are currently under wraps.',
			description: '??? ??? ???',
			stats: ['???', '???'],
			image: '/images/stealth.png',
			link: '#',
			color: 'text-white/50'
		}
	];
</script>

<div
	class="selection:bg-coral-500/30 selection:text-coral-100 relative min-h-screen overflow-x-hidden bg-[#020408] text-white"
>
	<div
		class="pointer-events-none fixed inset-0 z-10 opacity-[0.03]"
		style="background-image: url('data:image/svg+xml,%3Csvg viewBox=%220 0 200 200%22 xmlns=%22http://www.w3.org/2000/svg%22%3E%3Cfilter id=%22noise%22%3E%3CfeTurbulence type=%22fractalNoise%22 baseFrequency=%220.65%22 numOctaves=%223%22 stitchTiles=%22stitch%22/%3E%3C/filter%3E%3Crect width=%22100%25%22 height=%22100%25%22 filter=%22url(%23noise)%22 opacity=%221%22/%3E%3C/svg%3E');"
	></div>

	<CoralDrifters />
	<IslandNav />

	<main class="relative z-20 mx-auto max-w-7xl px-6 pt-32 pb-64 md:px-12">
		<!-- Header -->
		<div
			class="mb-32 space-y-6 text-center"
			use:viewport={{ once: true }}
			onviewportenter={() => (visible = true)}
		>
			{#if visible}
				<h1
					in:fly={{ y: 30, duration: 1000, easing: cubicOut }}
					class="font-display text-6xl font-medium tracking-tight md:text-8xl"
				>
					<span class="bg-gradient-to-r from-white to-white/50 bg-clip-text text-transparent"
						>Our Portfolio.</span
					>
				</h1>
				<p
					in:fly={{ y: 30, duration: 1000, delay: 200, easing: cubicOut }}
					class="mx-auto max-w-2xl text-xl leading-relaxed font-light text-white/60"
				>
					Diverse ventures. Unified by design, technology, and impact.
				</p>
			{/if}
		</div>

		<!-- Projects List -->
		<div class="space-y-32">
			{#each projects as project, i}
				<section
					class="group relative"
					use:viewport={{ once: true }}
					onviewportenter={(e: Event) => {
						// Simple visibility toggle for each section could be added here
						(e.target as HTMLElement).dataset.visible = 'true';
					}}
				>
					<!-- Alternating Layout -->
					<div
						class="flex flex-col {i % 2 === 0
							? 'md:flex-row'
							: 'md:flex-row-reverse'} items-center gap-12 md:gap-24"
					>
						<!-- Image Side -->
						<div class="relative w-full md:w-3/5">
							<div
								class="relative aspect-[16/9] overflow-hidden rounded-3xl border border-white/10 bg-white/5 shadow-2xl backdrop-blur-sm transition-transform duration-500 ease-out group-hover:scale-[1.01]"
							>
								<img
									src={project.image}
									alt={project.name}
									class="h-full w-full object-cover opacity-90 transition-opacity duration-500 group-hover:opacity-100"
								/>
								<!-- Glass Sheen -->
								<div
									class="pointer-events-none absolute inset-0 bg-gradient-to-tr from-white/5 to-transparent opacity-0 transition-opacity duration-500 group-hover:opacity-100"
								></div>
							</div>
							<!-- Decorative Coral Branch growing on the card -->
							<div
								class="pointer-events-none absolute z-20 opacity-80 mix-blend-overlay transition-transform duration-700 ease-out group-hover:scale-110 {i %
									2 ===
								0
									? '-bottom-12 -left-12 rotate-[-15deg]'
									: '-right-12 -bottom-12 rotate-[15deg]'}"
							>
								<CoralBranch
									variant={(i % 4) + 1}
									class="text-coral-400 h-72 w-72 brightness-125 saturate-150"
								/>
							</div>

							<!-- Second layer for depth -->
							<div
								class="pointer-events-none absolute z-10 opacity-40 mix-blend-color-dodge blur-[2px] {i %
									2 ===
								0
									? '-bottom-8 -left-8 rotate-[-5deg]'
									: '-right-8 -bottom-8 rotate-[5deg]'}"
							>
								<CoralBranch variant={((i + 1) % 4) + 1} class="text-coral-500 h-72 w-72" />
							</div>
						</div>

						<!-- Content Side -->
						<div class="w-full space-y-8 md:w-2/5">
							<div class="space-y-4">
								<h2
									class="font-display text-4xl font-bold tracking-tight md:text-5xl {project.color}"
								>
									{project.name}
								</h2>
								<p class="text-lg leading-snug font-medium text-white/90">{project.tagline}</p>
								<p class="leading-relaxed text-white/60">{project.description}</p>
							</div>

							<!-- Stats -->
							<div class="grid grid-cols-1 gap-4 border-t border-white/10 pt-4">
								{#each project.stats as stat}
									<div class="flex items-center gap-3">
										<div class="bg-coral-500 h-1.5 w-1.5 rounded-full"></div>
										<span class="font-mono text-sm text-white/70">{stat}</span>
									</div>
								{/each}
							</div>

							{#if project.link !== '#'}
								<div class="pt-4">
									<Button variant="ghost" href={project.link}>Visit Site</Button>
								</div>
							{/if}
						</div>
					</div>
				</section>
			{/each}
		</div>
	</main>

	<!-- Bottom Coral Field -->
	<div class="pointer-events-auto fixed right-0 bottom-0 left-0 z-0 h-[60vh] opacity-50">
		<CoralField />
	</div>
</div>

<style>
	:global(.font-display) {
		font-family: 'SF Pro Display', 'Inter', system-ui, sans-serif;
	}
</style>
