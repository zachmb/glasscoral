<script lang="ts">
	import { onMount } from 'svelte';
	import { fly } from 'svelte/transition';
	import { cubicOut } from 'svelte/easing';
	import { Play, FileText, Terminal, LayoutGrid, MoveRight } from 'lucide-svelte';

	/*
	  IMPORTS
	*/
	import IslandNav from '$lib/components/IslandNav.svelte';
	import CoralDrifters from '$lib/components/CoralDrifters.svelte';
	import StatsTicker from '$lib/components/StatsTicker.svelte';
	import StudioShowcase from '$lib/components/StudioShowcase.svelte';
	import CoralBranch from '$lib/components/CoralBranch.svelte';
	import CoralField from '$lib/components/CoralField.svelte';
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

	// State for Sections Visibility
	let visibleSections = $state({
		hero: false,
		stats: false,
		studios: false,
		methodology: false,
		footer: false
	});

	// Step State for Methodology
	let activeStep = $state(0);

	function stepInView(element: HTMLElement, index: number) {
		const observer = new IntersectionObserver(
			(entries) => {
				if (entries[0].isIntersecting) {
					activeStep = index;
				}
			},
			{ threshold: 0.5, rootMargin: '-20% 0px -20% 0px' }
		);

		observer.observe(element);
		return {
			destroy() {
				observer.disconnect();
			}
		};
	}

	// Animation Params
	const flyParams = { y: 30, duration: 1000, easing: cubicOut };
</script>

<!-- Global Scene Setup -->
<div
	class="selection:bg-coral-500/30 selection:text-coral-100 min-h-screen overflow-x-hidden bg-[#020408] text-white"
>
	<!-- Static Noise Layer -->
	<div
		class="pointer-events-none fixed inset-0 z-10 opacity-[0.03]"
		style="background-image: url('data:image/svg+xml,%3Csvg viewBox=%220 0 200 200%22 xmlns=%22http://www.w3.org/2000/svg%22%3E%3Cfilter id=%22noise%22%3E%3CfeTurbulence type=%22fractalNoise%22 baseFrequency=%220.65%22 numOctaves=%223%22 stitchTiles=%22stitch%22/%3E%3C/filter%3E%3Crect width=%22100%25%22 height=%22100%25%22 filter=%22url(%23noise)%22 opacity=%221%22/%3E%3C/svg%3E');"
	></div>

	<CoralDrifters />
	<IslandNav />

	<main class="relative z-20">
		<!-- Section 1: The Hero -->
		<section
			class="relative flex min-h-screen flex-col items-center justify-center px-6 pt-40 pb-64 text-center"
			use:viewport={{ once: true }}
			onviewportenter={() => (visibleSections.hero = true)}
		>
			<div class="text-coral-400 mb-6 font-mono text-sm tracking-widest uppercase opacity-80">
				/// Built by Ex-Founders >>>
			</div>

			{#if visibleSections.hero}
				<div in:fly={flyParams} class="mx-auto max-w-5xl space-y-8">
					<h1 class="font-display text-5xl leading-[1.1] font-medium tracking-tight md:text-8xl">
						We build, iterate <br />
						& scale
						<span
							class="via-coral-300 bg-gradient-to-r from-white to-white/50 bg-clip-text pl-4 text-transparent italic"
							>software companies.</span
						>
					</h1>

					<p class="mx-auto max-w-[60ch] text-xl leading-relaxed font-light text-white/60">
						A tech company mainly focused on building, acquiring and managing cool projects.
					</p>

					<div class="flex flex-col items-center justify-center gap-4 pt-8 sm:flex-row">
						<Button variant="coral">Join The Reef</Button>
						<Button variant="ghost">Enter The Trench</Button>
					</div>
				</div>

				<!-- Floating Kelp Decorations -->
				<CoralBranch
					variant={5}
					sway={true}
					class="pointer-events-none absolute bottom-[-100px] left-[-10%] z-0 h-[600px] w-[300px] rotate-12 opacity-20 blur-sm"
				/>
				<CoralBranch
					variant={5}
					sway={true}
					class="pointer-events-none absolute top-[20%] right-[-5%] z-0 h-[500px] w-[250px] -rotate-6 opacity-15 blur-sm"
				/>
			{/if}
		</section>

		<!-- Section 2: Stats / Vision -->
		<section
			use:viewport={{ once: true }}
			onviewportenter={() => (visibleSections.stats = true)}
			class="relative z-30"
		>
			{#if visibleSections.stats}
				<StatsTicker />
			{/if}
		</section>

		<!-- Section 3: Studio Showcase -->
		<section
			id="studios"
			class="relative flex flex-col items-center px-6 py-64"
			use:viewport={{ once: true }}
			onviewportenter={() => (visibleSections.studios = true)}
		>
			<!-- Decoration -->
			<CoralBranch
				variant={5}
				sway={true}
				class="pointer-events-none absolute top-0 left-0 z-0 h-[800px] w-[400px] -rotate-12 opacity-30"
			/>

			{#if visibleSections.studios}
				<div in:fly={flyParams} class="w-full max-w-7xl space-y-24">
					<!-- The Reef (Public Startups) -->
					<div class="flex flex-col gap-12 lg:flex-row">
						<div class="shrink-0 space-y-6 lg:w-1/4">
							<h2 class="font-display text-6xl font-bold tracking-tighter text-white">
								THE <br /> REEF
							</h2>
							<div class="bg-coral-500 h-px w-20"></div>
							<p class="text-white/60">
								Our publicly visible ecosystem. Ventures scaling in the light.
							</p>
						</div>
						<div class="lg:w-3/4">
							<StudioShowcase />
						</div>
					</div>

					<!-- The Trench (Stealth Startups) -->
					<div
						class="relative overflow-hidden rounded-3xl border border-white/10 bg-white/5 p-12 backdrop-blur-md"
					>
						<div
							class="flex flex-col items-start gap-8 md:flex-row md:items-center md:justify-between"
						>
							<div class="space-y-4">
								<h2 class="font-display text-4xl font-bold text-white">GLASS TRENCH</h2>
								<p class="text-coral-400 font-mono">/// STEALTH INCUBATION >>></p>
								<p class="max-w-xl text-white/60">
									The stealth startups we own. Hidden from the surface, building the next wave of
									infrastructure.
								</p>
							</div>
							<Button variant="ghost" class="shrink-0">Enter The Trench</Button>
						</div>
					</div>
				</div>
			{/if}
		</section>

		<!-- Section 4: Methodology -->
		<section
			id="methodology"
			class="px-6 py-64"
			use:viewport={{ once: true }}
			onviewportenter={() => (visibleSections.methodology = true)}
		>
			<!-- Background Decoration -->
			<CoralBranch
				variant={5}
				sway={true}
				class="pointer-events-none absolute top-[10%] left-[-5%] z-0 h-[700px] w-[350px] rotate-6 opacity-20 mix-blend-screen blur-sm"
			/>
			{#if visibleSections.methodology}
				<div in:fly={flyParams} class="mx-auto grid max-w-7xl items-start gap-20 md:grid-cols-2">
					<!-- Steps List (Scrollable) -->
					<div class="space-y-32 py-20 pb-64">
						<div class="space-y-4">
							<span class="text-coral-400 font-mono text-sm tracking-widest uppercase"
								>/// ABOUT US | OUR VISION >>></span
							>
							<h2 class="font-display text-5xl font-bold tracking-tight md:text-6xl">
								We build products that matter.
							</h2>
							<p class="text-xl text-white/60">
								From zero to scale, we obsess over every detail that drives growth. A studio mainly
								focused on building, acquiring and managing cool projects.
							</p>
						</div>

						<div class="relative ml-4 space-y-48 border-l border-white/10 pl-12">
							<!-- Step 0 -->
							<div
								class="relative transition-opacity duration-500 {activeStep === 0
									? 'opacity-100'
									: 'opacity-30'}"
								use:stepInView={0}
							>
								<div
									class="absolute top-1 -left-[53.5px] z-10 flex h-7 w-7 items-center justify-center rounded-full border bg-[#020408] transition-all duration-500 {activeStep ===
									0
										? 'border-coral-500 scale-125'
										: 'scale-100 border-white/20'}"
								>
									{#if activeStep === 0}
										<div
											class="bg-coral-500 h-2 w-2 rounded-full shadow-[0_0_10px_rgba(255,107,107,0.8)]"
											in:fly={{ duration: 300 }}
										></div>
									{/if}
								</div>
								<h3 class="mb-2 text-2xl font-bold text-white">Ideation</h3>
								<p class="text-lg text-white/50">
									We start with a problem. Deep research, market analysis, and a relentless focus on
									value.
								</p>
							</div>

							<!-- Step 1 -->
							<div
								class="relative transition-opacity duration-500 {activeStep === 1
									? 'opacity-100'
									: 'opacity-30'}"
								use:stepInView={1}
							>
								<div
									class="absolute top-1 -left-[53.5px] z-10 flex h-7 w-7 items-center justify-center rounded-full border bg-[#020408] transition-all duration-500 {activeStep ===
									1
										? 'border-coral-500 scale-125'
										: 'scale-100 border-white/20'}"
								>
									{#if activeStep === 1}
										<div
											class="bg-coral-500 h-2 w-2 rounded-full shadow-[0_0_10px_rgba(255,107,107,0.8)]"
											in:fly={{ duration: 300 }}
										></div>
									{/if}
								</div>
								<h3 class="mb-2 text-2xl font-bold text-white">Development</h3>
								<p class="text-lg text-white/50">
									Rapid prototyping. We ship fast, iterate faster, and build with scalability in
									mind.
								</p>
							</div>

							<!-- Step 2 -->
							<div
								class="relative transition-opacity duration-500 {activeStep === 2
									? 'opacity-100'
									: 'opacity-30'}"
								use:stepInView={2}
							>
								<div
									class="absolute top-1 -left-[53.5px] z-10 flex h-7 w-7 items-center justify-center rounded-full border bg-[#020408] transition-all duration-500 {activeStep ===
									2
										? 'border-coral-500 scale-125'
										: 'scale-100 border-white/20'}"
								>
									{#if activeStep === 2}
										<div
											class="bg-coral-500 h-2 w-2 rounded-full shadow-[0_0_10px_rgba(255,107,107,0.8)]"
											in:fly={{ duration: 300 }}
										></div>
									{/if}
								</div>
								<h3 class="mb-2 text-2xl font-bold text-white">Growth</h3>
								<p class="text-lg text-white/50">
									Leveraging our network. We push for mass adoption through organic and viral
									channels.
								</p>
							</div>
						</div>
					</div>

					<!-- Visual (Sticky Right) -->
					<div class="sticky top-32 hidden h-[80vh] items-center justify-center md:flex">
						<div
							class="relative flex aspect-[4/5] w-full max-w-md flex-col items-center justify-center overflow-hidden rounded-3xl border border-white/10 bg-white/5 shadow-2xl backdrop-blur-md"
						>
							<div
								class="from-coral-500/10 absolute inset-0 bg-gradient-to-br to-transparent"
							></div>

							<!-- Animated Content based on activeStep -->
							{#key activeStep}
								<div
									in:fly={{ y: 20, duration: 400 }}
									class="flex flex-col items-center p-8 text-center"
								>
									<Terminal class="text-coral-400 mb-6" size={64} />
									<h4 class="mb-2 font-display text-2xl font-bold text-white">
										{#if activeStep === 0}
											Thesis Validation
										{:else if activeStep === 1}
											MVP Construction
										{:else}
											Market Penetration
										{/if}
									</h4>
									<p class="px-4 font-mono text-sm text-white/40">
										{#if activeStep === 0}
											Analyze. Hypothesize. Verify.
										{:else if activeStep === 1}
											Code. Deploy. Iterate.
										{:else}
											Scale. optimize. Dominate.
										{/if}
									</p>
								</div>
							{/key}
						</div>
					</div>
				</div>
			{/if}
		</section>

		<!-- Section 6: Footer (The Titan Brand) -->
		<footer
			class="relative flex min-h-[50vh] flex-col items-center justify-end overflow-hidden pt-32 pb-4"
			use:viewport={{ once: true }}
			onviewportenter={() => (visibleSections.footer = true)}
		>
			<div class="z-20 mb-12 text-center">
				<p class="mb-4 font-mono text-sm tracking-widest text-white/40 uppercase">GlassCoral</p>
				<p class="mx-auto max-w-md px-6 text-lg text-white/60">
					Building the next generation of AI-native companies.
				</p>
			</div>

			<div class="relative z-20 w-full overflow-hidden px-2">
				<h1
					class="w-full bg-gradient-to-t from-white/80 to-white/20 bg-clip-text text-center font-display text-[14vw] leading-[0.7] font-bold tracking-tighter text-transparent select-none"
				>
					GLASSCORAL
				</h1>
			</div>
		</footer>

		<!-- Footer Coral Field (Unconstrained, massive height) -->
		<div class="pointer-events-auto absolute right-0 bottom-0 left-0 z-0 h-[150vh] overflow-hidden">
			<CoralField />
		</div>
	</main>
</div>

<style>
	:global(.font-display) {
		font-family: 'SF Pro Display', 'Inter', system-ui, sans-serif;
	}
</style>
