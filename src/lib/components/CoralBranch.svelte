<script lang="ts">
	import { onMount } from 'svelte';
	import { spring } from 'svelte/motion';

	let { variant = 4, class: className = '', ...rest } = $props();

	// SVG Path Logic
	const paths = {
		// New Variant 4: Single line splitting into two
		// M125,500 = Start bottom center
		// C... = Curving up to mid point
		// M... = Second branch splitting off
		4: 'M125,500 C125,500 125,400 125,300 C125,200 150,150 200,100 M125,300 C125,300 125,250 80,200',

		1: 'M10,400 C10,400 20,300 80,250 C140,200 120,100 180,50 M80,250 C80,250 40,200 40,150 M120,100 C120,100 160,150 200,120',
		2: 'M0,100 C50,100 100,50 150,80 C200,110 250,20 300,50 M150,80 C150,80 180,120 220,100',
		3: 'M50,150 C50,150 70,100 60,50 C50,0 90,20 100,0 M60,50 C60,50 30,40 20,20'
	};

	let pathLength = $state(0);

	// Scroll State
	let scrollY = $state(0);
	let innerHeight = $state(0);
	let element: HTMLElement;

	// Spring for smooth scrubbing (lag effect)
	const progress = spring(0, {
		stiffness: 0.05,
		damping: 0.5
	});

	// Calculate Progress based on scroll position - using a derived effect
	$effect(() => {
		if (element) {
			const bounds = element.getBoundingClientRect();

			// Range where animation happens:
			// Start: When element is slightly visible from bottom (90% change here)
			// End: When element is significantly up the page (20% from top)
			const startOffset = innerHeight * 0.9;
			const endOffset = innerHeight * 0.2;

			// Top position relative to effective viewport
			const top = bounds.top;

			let p = (startOffset - top) / (startOffset - endOffset);

			// Clamp
			p = Math.max(0, Math.min(1, p));

			progress.set(p);
		}
	});

	let pathElement: SVGPathElement;

	onMount(() => {
		if (pathElement) {
			pathLength = pathElement.getTotalLength();
		}
	});
</script>

<svelte:window bind:scrollY bind:innerHeight />

<div bind:this={element} class="pointer-events-none {className}" {...rest}>
	<svg viewBox="0 0 250 500" class="h-full w-full overflow-visible" fill="none">
		<defs>
			<linearGradient
				id="coral-branch-grad"
				x1="0"
				y1="100%"
				x2="100%"
				y2="0%"
				gradientUnits="userSpaceOnUse"
			>
				<stop stop-color="#FFAD91" />
				<stop offset="1" stop-color="#FF5733" />
			</linearGradient>

			<filter id="glow-branch" x="-50%" y="-50%" width="200%" height="200%">
				<feGaussianBlur stdDeviation="4" result="coloredBlur" />
				<feMerge>
					<feMergeNode in="coloredBlur" />
					<feMergeNode in="SourceGraphic" />
				</feMerge>
			</filter>
		</defs>

		<path
			bind:this={pathElement}
			d={paths[variant as keyof typeof paths] || paths[4]}
			stroke="url(#coral-branch-grad)"
			stroke-width="4"
			stroke-linecap="round"
			stroke-linejoin="round"
			filter="url(#glow-branch)"
			stroke-dasharray={pathLength}
			stroke-dashoffset={pathLength - $progress * pathLength}
			class="opacity-80"
		/>
	</svg>
</div>
