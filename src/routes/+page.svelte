<script lang="ts">
	import { onMount } from 'svelte';
	import { fly } from 'svelte/transition';
	import { cubicOut } from 'svelte/easing';
	import { Play, Crosshair, FileText, Terminal, LayoutGrid } from 'lucide-svelte';

	import IslandNav from '$lib/components/IslandNav.svelte';
	import CoralDrifters from '$lib/components/CoralDrifters.svelte';
	import DeepfakeChart from '$lib/components/DeepfakeChart.svelte';
	import BentoCard from '$lib/components/BentoCard.svelte';
	import TabSwitcher from '$lib/components/TabSwitcher.svelte';
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
			on:viewportenter={() => (visibleSections.hero = true)}
		>
			{#if visibleSections.hero}
				<div in:fly={flyParams} class="mx-auto max-w-4xl space-y-8">
					<h1 class="font-display text-7xl leading-[1.1] font-medium tracking-tight md:text-8xl">
						<span
							class="via-coral-300 bg-gradient-to-r from-white to-white/50 bg-clip-text text-transparent"
							>Clarity</span
						> starts here.
					</h1>

					<p class="mx-auto max-w-[65ch] text-xl leading-relaxed font-light text-white/70">
						AI-powered fact-checking and media verification from live meetings to articles, images,
						and videos.
					</p>

					<div class="flex items-center justify-center gap-4 pt-4">
						<Button variant="coral">Get Early Access</Button>
						<Button variant="ghost">
							<span class="flex items-center gap-2">
								Watch Demo <Play size={16} />
							</span>
						</Button>
					</div>
				</div>
			{/if}
		</section>

		<!-- Section 2: "See Through the Glass" -->
		<section
			class="flex flex-col items-center px-6 py-32"
			use:viewport={{ once: true }}
			on:viewportenter={() => (visibleSections.tabs = true)}
		>
			{#if visibleSections.tabs}
				<div in:fly={{ ...flyParams, delay: 200 }} class="w-full max-w-5xl space-y-12">
					<div class="space-y-4 text-center">
						<h2 class="font-display text-4xl tracking-tight md:text-5xl">See through the Glass</h2>
						<p class="mx-auto max-w-2xl text-white/60">
							Glass works in the background, scanning the world around you — text, images, audio,
							and video — to separate fact from fiction in real time.
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
			on:viewportenter={() => (visibleSections.data = true)}
		>
			{#if visibleSections.data}
				<div in:fly={flyParams} class="mx-auto grid max-w-5xl items-center gap-16 md:grid-cols-2">
					<div class="space-y-6">
						<h2 class="font-display text-4xl tracking-tight md:text-5xl">
							The truth shouldn't be blurry.
						</h2>
						<p class="text-lg leading-relaxed text-white/60">
							Every day, fake news, AI-generated content, and distorted information flood our feeds.
							It's harder than ever to know what's real and what's not. Glass gives you clarity.
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
			on:viewportenter={() => (visibleSections.grid = true)}
		>
			{#if visibleSections.grid}
				<div in:fly={flyParams} class="mx-auto max-w-6xl">
					<div class="grid h-auto grid-cols-1 gap-6 md:h-[600px] md:grid-cols-2">
						<BentoCard
							title="Sharp Accuracy"
							description="Every claim, every source, every detail checked against the strongest signals. No fluff—just truth."
						>
							<svelte:fragment slot="icon">
								<div class="w-fit rounded-xl border border-white/10 bg-white/5 p-3">
									<Crosshair class="text-coral-400" size={32} />
								</div>
							</svelte:fragment>
						</BentoCard>

						<BentoCard
							title="Transparent Proof"
							description="We don't just say what's real—we show you why. Evidence, context, and reasoning you can trust."
						>
							<svelte:fragment slot="icon">
								<div class="w-fit rounded-xl border border-white/10 bg-white/5 p-3">
									<FileText class="text-coral-400" size={32} />
								</div>
							</svelte:fragment>
						</BentoCard>

						<BentoCard
							title="Built for Speed"
							description="Get Answers in seconds. Forget hours of digging. Hit ⌘+/ to fact-check instantly—right where you are."
						>
							<svelte:fragment slot="icon">
								<div
									class="flex w-fit items-center gap-2 rounded-xl border border-white/10 bg-white/5 p-3"
								>
									<span class="text-coral-400 font-mono text-xl">⌘</span>
									<span class="font-mono text-xl text-white/50">/</span>
								</div>
							</svelte:fragment>
						</BentoCard>

						<BentoCard
							title="Always With You"
							description="Glass lives where you are—your browser, your chats, your workflow. No switching apps, no friction."
						>
							<svelte:fragment slot="icon">
								<div class="w-fit rounded-xl border border-white/10 bg-white/5 p-3">
									<LayoutGrid class="text-coral-400" size={32} />
								</div>
							</svelte:fragment>
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
			on:viewportenter={() => (visibleSections.steps = true)}
		>
			{#if visibleSections.steps}
				<div in:fly={flyParams} class="mx-auto grid max-w-6xl items-start gap-20 md:grid-cols-2">
					<!-- Steps List (Scrollable) -->
					<div class="space-y-64 py-20 pb-64">
						<div class="space-y-4">
							<span class="text-coral-400 font-mono text-sm tracking-wider uppercase"
								>How it works</span
							>
							<h2 class="font-display text-4xl tracking-tight md:text-5xl">
								Effortless verification.
							</h2>
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
								<h3 class="mb-2 text-xl font-bold text-white">Overlay</h3>
								<p class="text-white/50">
									Built for Speed. Select text on any site, hit <kbd
										class="rounded bg-white/10 px-1 font-sans text-white/80">⌘+/</kbd
									>, get results.
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
								<h3 class="mb-2 text-xl font-bold text-white">Dashboard</h3>
								<p class="text-white/50">
									Context on the go. See fact-checks, source bias, credibility instantly.
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
								<h3 class="mb-2 text-xl font-bold text-white">Stay in flow</h3>
								<p class="text-white/50">
									No tab-switching, no time wasted. Glass follows you around.
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
								<h3 class="mb-2 text-xl font-bold text-white">Works everywhere</h3>
								<p class="text-white/50">
									Glass follows you around. Social posts, news articles, PDFs.
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
											Scanning Page Content...
										{:else if activeStep === 1}
											Fact-Check Dashboard Active
										{:else if activeStep === 2}
											Seamless Integration Mode
										{:else}
											Universal Parsing Engine
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
			class="relative flex min-h-[50vh] flex-col items-center justify-end overflow-hidden pt-32 pb-0"
			use:viewport={{ once: true }}
			on:viewportenter={() => (visibleSections.footer = true)}
		>
			<div class="z-20 mb-12 text-center">
				<p class="mb-4 font-mono text-sm tracking-widest text-white/40 uppercase">GlassCoral AI</p>
				<p class="mx-auto max-w-md px-6 text-lg text-white/60">
					AI-powered fact-checking and media verification — from live meetings to articles, images,
					and videos.
				</p>
			</div>

			<h1
				class="pointer-events-none translate-y-[10%] bg-gradient-to-t from-white/20 to-white/5 bg-clip-text text-[13vw] leading-[0.75] font-bold tracking-tighter text-transparent select-none"
			>
				GLASSCORAL
			</h1>
		</footer>
	</main>
</div>

<style>
	:global(.font-display) {
		font-family: 'SF Pro Display', 'Inter', system-ui, sans-serif;
	}
</style>
