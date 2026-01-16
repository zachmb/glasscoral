<script lang="ts">
	import { onMount } from 'svelte';
	import { fly } from 'svelte/transition';
	import { cubicOut } from 'svelte/easing';
	import { Play, FileText, Terminal, LayoutGrid } from 'lucide-svelte';

	import IslandNav from '$lib/components/IslandNav.svelte';
	import CoralDrifters from '$lib/components/CoralDrifters.svelte';
	import DeepfakeChart from '$lib/components/DeepfakeChart.svelte';
	import BentoCard from '$lib/components/BentoCard.svelte';
	import TabSwitcher from '$lib/components/TabSwitcher.svelte';
	import Button from '$lib/components/Button.svelte';
	import CoralBranch from '$lib/components/CoralBranch.svelte';
	import CoralField from '$lib/components/CoralField.svelte';
	import CoralIcon from '$lib/components/CoralIcon.svelte';

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
			observer = new IntersectionObserver(handleIntersect, { threshold: 0.2 });
			observer.observe(element);
			return () => observer.disconnect();
		});
	}

	// State for Sections Visibility
	let visibleSections = $state({
		hero: false,
		tabs: false,
		data: false,
		grid: false,
		steps: false,
		footer: false
	});

	// Tab State
	let activeTab = $state('Conversations');

	// Step State
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
			class="relative flex min-h-screen flex-col items-center justify-center px-6 text-center"
			use:viewport={{ once: true }}
			onviewportenter={() => (visibleSections.hero = true)}
		>
			{#if visibleSections.hero}
				<div in:fly={flyParams} class="mx-auto max-w-4xl space-y-8">
					<h1 class="font-display text-7xl leading-[1.1] font-medium tracking-tight md:text-8xl">
						<span
							class="via-coral-300 bg-gradient-to-r from-white to-white/50 bg-clip-text text-transparent"
							>Cultivating</span
						> the next generation of AI.
					</h1>

					<p class="mx-auto max-w-[65ch] text-xl leading-relaxed font-light text-white/70">
						A studio building the future of software. From gaming to digital wellness and social OS.
					</p>

					<div class="flex items-center justify-center gap-4 pt-4">
						<Button variant="ghost">
							<span class="flex items-center gap-2">
								Our Philosophy <Play size={16} />
							</span>
						</Button>
					</div>
				</div>
			{/if}
		</section>

		<!-- Section 2: "See Through the Glass" -->
		<section
			class="relative flex flex-col items-center px-6 py-32"
			use:viewport={{ once: true }}
			onviewportenter={() => (visibleSections.tabs = true)}
		>
			<!-- MAIN SPLITTING CORAL BRANCH (Right Side) -->
			<CoralBranch
				variant={4}
				class="pointer-events-none absolute top-[-50px] right-[-10%] z-0 h-[600px] w-[300px] rotate-12 opacity-40"
			/>
			{#if visibleSections.tabs}
				<div in:fly={{ ...flyParams, delay: 200 }} class="w-full max-w-5xl space-y-12">
					<div class="space-y-4 text-center">
						<h2 class="font-display text-4xl tracking-tight md:text-5xl">Our Focus Areas</h2>
						<p class="mx-auto max-w-2xl text-white/60">
							We don't just build products; we build ecosystems. Our ventures span gaming,
							productivity, and social operating systems.
						</p>
					</div>

					<div class="flex justify-center">
						<TabSwitcher bind:activeTab />
					</div>

					<div
						class="relative flex aspect-video w-full items-center justify-center overflow-hidden rounded-3xl border border-white/10 bg-white/5 shadow-2xl backdrop-blur-sm"
					>
						<!-- Dynamic Content based on generic ActiveTab -->
						<p class="text-lg tracking-widest text-white/30 uppercase">{activeTab} Demo Mockup</p>
						<div
							class="absolute inset-0 bg-gradient-to-t from-[#020408] via-transparent to-transparent opacity-50"
						></div>
					</div>
				</div>
			{/if}
		</section>

		<!-- Section 3: The Data Story -->
		<section
			class="px-6 py-32"
			use:viewport={{ once: true }}
			onviewportenter={() => (visibleSections.data = true)}
		>
			{#if visibleSections.data}
				<div in:fly={flyParams} class="mx-auto grid max-w-5xl items-center gap-16 md:grid-cols-2">
					<div class="space-y-6">
						<h2 class="font-display text-4xl tracking-tight md:text-5xl">Impact at Scale.</h2>
						<p class="text-lg leading-relaxed text-white/60">
							Across our portfolio, we reach millions of users daily. From 120M+ page views in
							gaming to 10M+ interactions in digital wellness presence.
						</p>
					</div>
					<div class="relative">
						<DeepfakeChart />
						<!-- Glow under chart -->
						<div
							class="pointer-events-none absolute -inset-10 z-[-1] rounded-full bg-[#FF6B6B] opacity-10 blur-[100px]"
						></div>
					</div>
				</div>
			{/if}
		</section>

		<!-- Section 4: "Why Glass" (Bento Grid) -->
		<section
			id="features"
			class="px-6 py-32"
			use:viewport={{ once: true }}
			onviewportenter={() => (visibleSections.grid = true)}
		>
			{#if visibleSections.grid}
				<div in:fly={flyParams} class="mx-auto max-w-6xl">
					<div class="grid h-auto grid-cols-1 gap-6 md:h-[600px] md:grid-cols-2 md:grid-rows-2">
						<!-- Card 1: Precision Engineering (Top Left) -->
						<BentoCard
							title="Precision Engineering"
							description="We obsess over the details. Performance, aesthetics, and user experience are never compromised."
							class="md:col-start-1 md:row-start-1"
						>
							<div class="w-fit rounded-xl border border-white/10 bg-white/5 p-3">
								<CoralIcon class="text-coral-400" size={32} />
							</div>
						</BentoCard>

						<!-- Card 2: Built for Scale (Bottom Left) -->
						<BentoCard
							title="Built for Scale"
							description="Our infrastructure is designed to handle millions of concurrent users from day one."
							class="md:col-start-1 md:row-start-2"
						>
							<div
								class="flex w-fit items-center gap-2 rounded-xl border border-white/10 bg-white/5 p-3"
							>
								<span class="text-coral-400 font-mono text-xl">130</span>
								<span class="font-mono text-xl text-white/50">M+</span>
							</div>
						</BentoCard>

						<!-- Card 3: Studio Model (Right Tall) -->
						<BentoCard
							title="Studio Model"
							description="Shared resources, shared knowledge. We leverage our collective expertise to accelerate every venture we launch."
							class="h-full md:col-start-2 md:row-span-2 md:row-start-1"
						>
							<div class="flex items-center gap-4">
								<div class="w-fit rounded-xl border border-white/10 bg-white/5 p-3">
									<FileText class="text-coral-400" size={32} />
								</div>
								<div class="w-fit rounded-xl border border-white/10 bg-white/5 p-3 opacity-50">
									<LayoutGrid class="text-white" size={32} />
								</div>
							</div>
						</BentoCard>
					</div>
				</div>
			{/if}
		</section>

		<!-- Section 5: "How Glass Works" -->
		<section
			id="how-it-works"
			class="px-6 py-32"
			use:viewport={{ once: true }}
			onviewportenter={() => (visibleSections.steps = true)}
		>
			{#if visibleSections.steps}
				<div in:fly={flyParams} class="mx-auto grid max-w-6xl items-start gap-20 md:grid-cols-2">
					<!-- Steps List (Scrollable) -->
					<div class="space-y-64 py-20 pb-64">
						<div class="space-y-4">
							<span class="text-coral-400 font-mono text-sm tracking-wider uppercase"
								>Our Process</span
							>
							<h2 class="font-display text-4xl tracking-tight md:text-5xl">Incubation to Exit.</h2>
						</div>

						<div class="relative ml-4 space-y-64 border-l border-white/10 pl-12">
							<!-- Step 0 -->
							<div
								class="relative transition-opacity duration-500 {activeStep === 0
									? 'opacity-100'
									: 'opacity-30'}"
								use:stepInView={0}
							>
								<div
									class="absolute top-1 -left-[55px] z-10 flex h-8 w-8 items-center justify-center rounded-full border bg-[#020408] transition-all duration-500 {activeStep ===
									0
										? 'border-coral-500/50 scale-110'
										: 'scale-100 border-white/10'}"
								>
									{#if activeStep === 0}
										<div class="bg-coral-500 h-3 w-3 rounded-full" in:fly={{ duration: 300 }}></div>
									{/if}
								</div>
								<h3 class="mb-2 text-xl font-bold text-white">Ideation</h3>
								<p class="text-white/50">
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
									class="absolute top-1 -left-[55px] z-10 flex h-8 w-8 items-center justify-center rounded-full border bg-[#020408] transition-all duration-500 {activeStep ===
									1
										? 'border-coral-500/50 scale-110'
										: 'scale-100 border-white/10'}"
								>
									{#if activeStep === 1}
										<div class="bg-coral-500 h-3 w-3 rounded-full" in:fly={{ duration: 300 }}></div>
									{/if}
								</div>
								<h3 class="mb-2 text-xl font-bold text-white">Development</h3>
								<p class="text-white/50">
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
									class="absolute top-1 -left-[55px] z-10 flex h-8 w-8 items-center justify-center rounded-full border bg-[#020408] transition-all duration-500 {activeStep ===
									2
										? 'border-coral-500/50 scale-110'
										: 'scale-100 border-white/10'}"
								>
									{#if activeStep === 2}
										<div class="bg-coral-500 h-3 w-3 rounded-full" in:fly={{ duration: 300 }}></div>
									{/if}
								</div>
								<h3 class="mb-2 text-xl font-bold text-white">Growth</h3>
								<p class="text-white/50">
									Leveraging our network. We push for mass adoption through organic and viral
									channels.
								</p>
							</div>

							<!-- Step 3 -->
							<div
								class="relative transition-opacity duration-500 {activeStep === 3
									? 'opacity-100'
									: 'opacity-30'}"
								use:stepInView={3}
							>
								<div
									class="absolute top-1 -left-[55px] z-10 flex h-8 w-8 items-center justify-center rounded-full border bg-[#020408] transition-all duration-500 {activeStep ===
									3
										? 'border-coral-500/50 scale-110'
										: 'scale-100 border-white/10'}"
								>
									{#if activeStep === 3}
										<div class="bg-coral-500 h-3 w-3 rounded-full" in:fly={{ duration: 300 }}></div>
									{/if}
								</div>
								<h3 class="mb-2 text-xl font-bold text-white">Scale</h3>
								<p class="text-white/50">
									From a product to a platform. We expand, diversify, and dominate the vertical.
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
								<div in:fly={{ y: 20, duration: 400 }} class="flex flex-col items-center">
									<Terminal class="mb-4 text-white/20" size={64} />
									<p class="px-8 text-center font-mono text-sm text-white/30">
										{#if activeStep === 0}
											Validating Core Thesis
										{:else if activeStep === 1}
											Building the MVP
										{:else if activeStep === 2}
											Market Penetration
										{:else}
											Vertical Domination
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
					class="w-full bg-gradient-to-t from-white/20 to-white/5 bg-clip-text text-center font-display text-[14vw] leading-[0.7] font-bold tracking-tighter text-transparent opacity-50 mix-blend-overlay select-none"
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
