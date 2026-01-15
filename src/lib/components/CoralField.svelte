<script lang="ts">
	import { onMount } from 'svelte';
	import { spring } from 'svelte/motion';

	// ==========================================
	// 1. TYPES & INTERFACES
	// ==========================================

	type Vector2 = { x: number; y: number };

	type Layer = 'background' | 'midground' | 'foreground';

	type SpeciesType = 'staghorn' | 'fan' | 'tube' | 'grass' | 'brain';

	interface CoralStyle {
		fill: string;
		stroke: string;
		strokeWidth: number;
		ridgeColor: string;
		ridgeOpacity: number;
	}

	interface CoralEntity {
		id: string;
		type: SpeciesType;
		layer: Layer;
		x: number; // Percentage 0-100
		y: number; // Base Y position
		scale: number;
		rotation: number;

		// Paths
		bodyPath: string; // The main shape
		ridgePath: string; // The texture/detail overlay

		// Visuals
		style: CoralStyle;
		opacity: number;
		blur: number;
		zIndex: number;

		// Animation
		swaySpeed: number;
		swayOffset: number;
	}

	interface Bubble {
		id: string;
		x: number;
		y: number;
		r: number;
		speed: number;
		opacity: number;
	}

	// ==========================================
	// 2. CONFIGURATION & PALETTES
	// ==========================================

	const LAYERS = {
		background: { count: 40, scale: [0.3, 0.5], blur: 3, opacity: 0.3, z: 0 },
		midground: { count: 25, scale: [0.6, 0.9], blur: 0.5, opacity: 0.8, z: 10 },
		foreground: { count: 12, scale: [1.0, 1.4], blur: 0, opacity: 1.0, z: 20 }
	};

	const PALETTES = [
		{ fill: 'none', stroke: '#e63946', ridge: '#ffb3b3' }, // Red/Pink
		{ fill: 'none', stroke: '#2a9d8f', ridge: '#a8dadc' }, // Teal/Cyan
		{ fill: 'none', stroke: '#ffb703', ridge: '#ffe5ec' }, // Amber
		{ fill: 'none', stroke: '#8338ec', ridge: '#e0c3fc' }, // Purple
		{ fill: 'none', stroke: '#3a86ff', ridge: '#caf0f8' }, // Blue
		{ fill: 'none', stroke: '#fb8500', ridge: '#ffddd2' } // Orange
	];

	// ==========================================
	// 3. MATH HELPERS
	// ==========================================

	function degToRad(deg: number): number {
		return deg * (Math.PI / 180);
	}

	function randomRange(min: number, max: number): number {
		return Math.random() * (max - min) + min;
	}

	function randomInt(min: number, max: number): number {
		return Math.floor(Math.random() * (max - min + 1)) + min;
	}

	/**
	 * Creates a quadratic bezier string part
	 */
	function curveTo(cx: number, cy: number, x: number, y: number): string {
		return `Q ${cx.toFixed(1)} ${cy.toFixed(1)} ${x.toFixed(1)} ${y.toFixed(1)}`;
	}

	// ==========================================
	// 4. PROCEDURAL GENERATORS (The Core Logic)
	// ==========================================

	/**
	 * Generator 1: Staghorn Coral (Recursive Fractal)
	 * Creates a tree-like structure with "ridged" texture lines following the branches.
	 */
	function generateStaghorn(
		x: number,
		y: number,
		angle: number,
		len: number,
		width: number,
		depth: number
	): { body: string; ridges: string } {
		if (depth === 0) return { body: '', ridges: '' };

		// Calculate end point with organic curvature
		const curve = (Math.random() - 0.5) * 0.5;
		const x2 = x + Math.cos(angle + curve) * len;
		const y2 = y + Math.sin(angle + curve) * len;

		// Control point for Bezier
		const cx = (x + x2) / 2 + (Math.random() - 0.5) * len * 0.2;
		const cy = (y + y2) / 2;

		// Main Branch Path
		let body = `M ${x.toFixed(1)} ${y.toFixed(1)} ${curveTo(cx, cy, x2, y2)} `;

		// Ridge Texture: A dashed line running parallel or inside
		// We add a slight offset to the ridge so it looks like a highlight
		const rOffX = Math.cos(angle + Math.PI / 2) * (width * 0.3);
		const rOffY = Math.sin(angle + Math.PI / 2) * (width * 0.3);
		let ridges = `M ${(x + rOffX).toFixed(1)} ${(y + rOffY).toFixed(1)} ${curveTo(cx + rOffX, cy + rOffY, x2 + rOffX, y2 + rOffY)} `;

		// Recursion
		const branchCount = Math.random() > 0.7 ? 3 : 2;

		for (let i = 0; i < branchCount; i++) {
			// Spread angles based on depth (wider at bottom, narrower at top)
			const spread = 0.6 + 0.1 * depth;
			const newAngle = angle + (Math.random() * spread - spread / 2);
			const newLen = len * (0.7 + Math.random() * 0.2);
			const newWidth = width * 0.7;

			const child = generateStaghorn(x2, y2, newAngle, newLen, newWidth, depth - 1);
			body += child.body;
			ridges += child.ridges;
		}

		return { body, ridges };
	}

	/**
	 * Generator 2: Sea Fan (Grid/Web Structure)
	 * Creates a wide fan shape with cross-hatching.
	 */
	function generateSeaFan(x: number, y: number): { body: string; ridges: string } {
		let body = '';
		let ridges = '';

		const mainLimbs = 7;
		const height = 180 + Math.random() * 100;
		const fanWidthRad = Math.PI / 1.5; // 120 degrees
		const startAngle = -Math.PI / 2 - fanWidthRad / 2;

		// Store limb points to draw cross-webs later
		const limbPoints: Vector2[][] = [];

		// 1. Draw Main Ribs
		for (let i = 0; i <= mainLimbs; i++) {
			const progress = i / mainLimbs;
			const angle = startAngle + progress * fanWidthRad;
			const segments = 5;
			const segLen = height / segments;

			let cx = x;
			let cy = y;
			const currentLimb: Vector2[] = [{ x, y }];

			// Move up the limb
			for (let j = 0; j < segments; j++) {
				const wobble = (Math.random() - 0.5) * 0.2;
				const nx = cx + Math.cos(angle + wobble) * segLen;
				const ny = cy + Math.sin(angle + wobble) * segLen;

				body += `M ${cx.toFixed(1)} ${cy.toFixed(1)} L ${nx.toFixed(1)} ${ny.toFixed(1)} `;

				cx = nx;
				cy = ny;
				currentLimb.push({ x: cx, y: cy });
			}
			limbPoints.push(currentLimb);
		}

		// 2. Draw Webbing (Ridges)
		// Connect adjacent limbs
		for (let i = 0; i < limbPoints.length - 1; i++) {
			const limbA = limbPoints[i];
			const limbB = limbPoints[i + 1];

			// Connect roughly every other segment
			for (let j = 1; j < limbA.length; j++) {
				if (Math.random() > 0.3) {
					ridges += `M ${limbA[j].x.toFixed(1)} ${limbA[j].y.toFixed(1)} L ${limbB[j].x.toFixed(1)} ${limbB[j].y.toFixed(1)} `;
				}
			}
		}

		return { body, ridges };
	}

	/**
	 * Generator 3: Tube Sponge (Cylinders)
	 * Draws thick tubes with an ellipse at the top.
	 */
	function generateTubeSponge(x: number, y: number): { body: string; ridges: string } {
		let body = '';
		let ridges = '';
		const tubes = randomInt(3, 6);
		const clusterWidth = 40;

		for (let i = 0; i < tubes; i++) {
			const h = randomRange(60, 140);
			const w = randomRange(15, 25);
			const ox = x + (Math.random() - 0.5) * clusterWidth; // offset x

			// Left side curve
			const c1x = ox - w / 4;
			const c1y = y - h / 2;
			const topX = ox + (Math.random() - 0.5) * 10; // Slight lean
			const topY = y - h;

			// Draw tube body (A path that goes up, loops top, comes down)
			// We construct a path that looks like a tube
			body += `M ${ox - w / 2} ${y} `; // Bottom left
			body += `Q ${ox - w / 2} ${y - h / 2} ${topX - w / 2} ${topY} `; // Left wall
			body += `L ${topX + w / 2} ${topY} `; // Top Flat (or curve)
			body += `Q ${ox + w / 2} ${y - h / 2} ${ox + w / 2} ${y} `; // Right wall
			body += `Z `; // Close

			// Ridge: The "opening" at the top and vertical striations
			// Top ellipse
			ridges += `M ${topX - w / 2} ${topY} A ${w / 2} 4 0 0 1 ${topX + w / 2} ${topY} `;
			ridges += `M ${topX - w / 2} ${topY} A ${w / 2} 4 0 0 0 ${topX + w / 2} ${topY} `;

			// Vertical lines
			ridges += `M ${ox} ${y} Q ${ox} ${y - h / 2} ${topX} ${topY + 4} `;
		}

		return { body, ridges };
	}

	/**
	 * Generator 4: Sea Grass (Bezier Strips)
	 * Simple but effective sway.
	 */
	function generateSeaGrass(x: number, y: number): { body: string; ridges: string } {
		let body = '';
		const bladeCount = randomInt(8, 15);

		for (let i = 0; i < bladeCount; i++) {
			const h = randomRange(80, 160);
			const lean = randomRange(-40, 40);
			const ox = x + randomRange(-10, 10);

			// Simple quadratic curve
			// Control point
			const cpX = ox + lean / 2;
			const cpY = y - h / 2;
			const endX = ox + lean;
			const endY = y - h;

			body += `M ${ox.toFixed(1)} ${y} Q ${cpX.toFixed(1)} ${cpY.toFixed(1)} ${endX.toFixed(1)} ${endY.toFixed(1)} `;
		}
		// Grass doesn't really have ridges, but we can duplicate the path for a highlight
		return { body, ridges: body };
	}

	// ==========================================
	// 5. STATE & LOGIC
	// ==========================================

	let entities = $state<CoralEntity[]>([]);
	let bubbles = $state<Bubble[]>([]);
	let canvasWidth = $state(1000);
	let canvasHeight = $state(600);

	// Mouse Interaction State
	const mouseSpring = spring({ x: 0.5, y: 0.5 }, { stiffness: 0.05, damping: 0.25 });

	function handleMouseMove(e: MouseEvent) {
		const { clientX, clientY } = e;
		const { innerWidth, innerHeight } = window;
		mouseSpring.set({
			x: clientX / innerWidth,
			y: clientY / innerHeight
		});
	}

	// Bubble Spawner
	function spawnBubble() {
		const id = Math.random().toString(36);
		const x = Math.random() * 100;
		const r = Math.random() * 4 + 1;

		bubbles.push({
			id,
			x,
			y: 110, // Start below screen
			r,
			speed: 0.2 + Math.random() * 0.3,
			opacity: 0.3 + Math.random() * 0.4
		});
	}

	// Animation Loop for Bubbles
	function gameLoop() {
		// Update bubbles
		for (let i = bubbles.length - 1; i >= 0; i--) {
			const b = bubbles[i];
			b.y -= b.speed; // Move up
			b.x += Math.sin(b.y * 0.05) * 0.1; // Wiggle

			if (b.y < -10) {
				bubbles.splice(i, 1);
			}
		}

		if (Math.random() > 0.95) spawnBubble();

		requestAnimationFrame(gameLoop);
	}

	onMount(() => {
		const newEntities: CoralEntity[] = [];

		// Generation Pipeline
		(['background', 'midground', 'foreground'] as Layer[]).forEach((layerName) => {
			const config = LAYERS[layerName];

			for (let i = 0; i < config.count; i++) {
				// 1. Pick Species
				const rand = Math.random();
				let type: SpeciesType = 'staghorn';
				if (rand > 0.75) type = 'fan';
				else if (rand > 0.6) type = 'tube';
				else if (rand > 0.45) type = 'grass';

				// 2. Decide Placement (Bottom vs Sides)
				let x = Math.random() * 1000;
				let y = 600; // Bottom
				let rotation = randomRange(-5, 5);
				let scaleMult = 1;

				const placementRand = Math.random();
				let isSide = false;

				if (placementRand > 0.8) {
					// Left Wall
					x = 0;
					y = randomRange(300, 600); // Only lower half
					rotation = 90 + randomRange(-15, 15);
					isSide = true;
				} else if (placementRand > 0.6) {
					// Right Wall
					x = 1000;
					y = randomRange(300, 600); // Only lower half
					rotation = -90 + randomRange(-15, 15);
					isSide = true;
				}

				let paths = { body: '', ridges: '' };

				// Parametrization based on type
				switch (type) {
					case 'staghorn':
						// Side corals should be twistier/reach out
						const len = isSide ? 90 : 60;
						paths = generateStaghorn(x, y, -Math.PI / 2, len, 8, 5);
						break;
					case 'fan':
						paths = generateSeaFan(x, y);
						break;
					case 'tube':
						paths = generateTubeSponge(x, y);
						break;
					case 'grass':
						paths = generateSeaGrass(x, y);
						break;
				}

				// 3. Style Selection
				const palette = PALETTES[Math.floor(Math.random() * PALETTES.length)];
				const style: CoralStyle = {
					fill: type === 'tube' ? palette.stroke : 'none', // Tubes need fill
					stroke: palette.stroke,
					strokeWidth: type === 'grass' ? 2 : 4,
					ridgeColor: palette.ridge,
					ridgeOpacity: type === 'grass' ? 0.3 : 0.6
				};

				// 4. Transform Properties
				const scale = randomRange(config.scale[0], config.scale[1]);

				// 5. Create Entity
				newEntities.push({
					id: `${layerName}-${i}`,
					type,
					layer: layerName,
					x: (x / 1000) * 100, // Store as percentage
					y: (y / 600) * 100, // Store as percentage of height
					scale,
					rotation,
					bodyPath: paths.body,
					ridgePath: paths.ridges,
					style,
					opacity: config.opacity,
					blur: config.blur,
					zIndex: config.z + i,
					swaySpeed: randomRange(3, 8),
					swayOffset: randomRange(0, 100)
				});
			}
		});

		// Sort by Z-Index so foreground covers background
		newEntities.sort((a, b) => a.zIndex - b.zIndex);
		entities = newEntities;

		// Start Loops
		gameLoop();
	});
</script>

<svelte:window onmousemove={handleMouseMove} />

<div class="scene-container">
	<div class="ocean-bg"></div>
	<div class="vignette"></div>

	<div class="light-rays">
		<div class="ray ray-1"></div>
		<div class="ray ray-2"></div>
		<div class="ray ray-3"></div>
	</div>

	<svg class="coral-svg" viewBox="0 0 1000 600" preserveAspectRatio="xMidYMax slice">
		<defs>
			<filter id="glow-soft">
				<feGaussianBlur stdDeviation="2" result="coloredBlur" />
				<feMerge>
					<feMergeNode in="coloredBlur" />
					<feMergeNode in="SourceGraphic" />
				</feMerge>
			</filter>

			<filter id="water-distort">
				<feTurbulence type="fractalNoise" baseFrequency="0.01" numOctaves="2" result="warp" />
				<feDisplacementMap
					xChannelSelector="R"
					yChannelSelector="G"
					scale="10"
					in="SourceGraphic"
					in2="warp"
				/>
			</filter>
		</defs>

		{#each entities as coral (coral.id)}
			{@const mouseOffsetX =
				($mouseSpring.x - 0.5) *
				(coral.layer === 'foreground' ? -40 : coral.layer === 'midground' ? -20 : -5)}

			<g
				class="coral-group"
				style="
					transform-origin: 50% 100%;
					--sway-speed: {coral.swaySpeed}s;
					--sway-delay: -{coral.swayOffset}s;
					--base-scale: {coral.scale};
					--mouse-x: {mouseOffsetX}px;
				"
			>
				<g
					style="transform: translateX(calc({coral.x *
						10}px + var(--mouse-x))) translateY({coral.y * 6}px)"
				>
					<g class="sway-anim">
						<g
							style="transform: scale(var(--base-scale)) rotate({coral.rotation}deg); opacity: {coral.opacity}; filter: blur({coral.blur}px)"
						>
							<path
								d={coral.bodyPath}
								fill={coral.style.fill}
								fill-opacity="0.3"
								stroke={coral.style.stroke}
								stroke-width={coral.style.strokeWidth}
								stroke-linecap="round"
								stroke-linejoin="round"
							/>

							<path
								d={coral.ridgePath}
								fill="none"
								stroke={coral.style.ridgeColor}
								stroke-width="1.5"
								stroke-opacity={coral.style.ridgeOpacity}
								stroke-dasharray={coral.type === 'grass' ? '0' : '2 3'}
								stroke-linecap="round"
								class="mix-blend-overlay"
							/>

							{#if coral.layer === 'foreground' && coral.type !== 'grass'}
								<path
									d={coral.bodyPath}
									stroke={coral.style.stroke}
									stroke-width="8"
									stroke-opacity="0.1"
									filter="url(#glow-soft)"
									fill="none"
								/>
							{/if}
						</g>
					</g>
				</g>
			</g>
		{/each}

		{#each bubbles as b (b.id)}
			<circle
				cx={b.x * 10}
				cy={b.y * 6}
				r={b.r}
				fill="rgba(255,255,255,0.4)"
				stroke="rgba(255,255,255,0.6)"
				stroke-width="1"
			/>
		{/each}
	</svg>

	<div class="noise-overlay"></div>
</div>

<style>
	/* ==========================================
		CSS STYLING & ANIMATIONS
		==========================================
	*/

	/* Container setup */
	/* Container setup */
	.scene-container {
		position: relative;
		width: 100%;
		height: 100%;
		overflow: hidden;
		/* Mask to fade out the top 30% smoothly */
		-webkit-mask-image: linear-gradient(to bottom, transparent 0%, black 40%);
		mask-image: linear-gradient(to bottom, transparent 0%, black 40%);
		background-color: transparent;
		user-select: none;
	}

	/* Background Gradient (Transparent Top) */
	.ocean-bg {
		position: absolute;
		inset: 0;
		background: linear-gradient(to bottom, transparent 0%, #001219 20%, #000508 100%);
		z-index: 0;
	}

	/* Vignette */
	.vignette {
		position: absolute;
		inset: 0;
		background: radial-gradient(transparent 50%, rgba(0, 0, 0, 0.6) 100%);
		z-index: 50;
		pointer-events: none;
	}

	/* Noise Grain */
	.noise-overlay {
		position: absolute;
		inset: 0;
		z-index: 60;
		opacity: 0.05;
		pointer-events: none;
		background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 200 200' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='noiseFilter'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.65' numOctaves='3' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23noiseFilter)'/%3E%3C/svg%3E");
	}

	/* The SVG */
	.coral-svg {
		position: absolute;
		bottom: 0;
		left: 0;
		width: 100%;
		height: 100%;
		z-index: 10;
		/* Ensure smooth rendering */
		shape-rendering: geometricPrecision;
	}

	/* Sway Animation 
		Uses CSS variables set in the HTML for per-instance randomization
	*/
	.sway-anim {
		animation: sway var(--sway-speed) ease-in-out infinite;
		animation-delay: var(--sway-delay);
		transform-origin: 50% 100%; /* Pivot at bottom */
	}

	@keyframes sway {
		0%,
		100% {
			transform: rotate(-3deg) scale(1);
		}
		50% {
			transform: rotate(3deg) scale(1.02);
		}
	}

	/* Light Rays 
		Angled divs with gradients that move slowly
	*/
	.light-rays {
		position: absolute;
		top: 0;
		left: 0;
		right: 0;
		height: 100%;
		overflow: hidden;
		z-index: 5;
		pointer-events: none;
	}

	.ray {
		position: absolute;
		top: -20%;
		width: 15%;
		height: 150%;
		background: linear-gradient(180deg, rgba(255, 255, 255, 0.1) 0%, rgba(255, 255, 255, 0) 80%);
		transform: rotate(25deg);
		filter: blur(15px);
		opacity: 0.5;
		animation: ray-shift 10s infinite alternate ease-in-out;
	}

	.ray-1 {
		left: 20%;
		animation-duration: 12s;
	}
	.ray-2 {
		left: 50%;
		width: 20%;
		animation-duration: 15s;
		animation-delay: -5s;
	}
	.ray-3 {
		left: 80%;
		width: 10%;
		animation-duration: 18s;
		animation-delay: -2s;
	}

	@keyframes ray-shift {
		0% {
			transform: rotate(25deg) translateX(-20px);
			opacity: 0.3;
		}
		100% {
			transform: rotate(25deg) translateX(20px);
			opacity: 0.6;
		}
	}

	/* Utilities */
	.mix-blend-overlay {
		mix-blend-mode: overlay;
	}
</style>
