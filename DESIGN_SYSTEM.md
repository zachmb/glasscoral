# GlassCoral Design System: "Forged Fluidity" & "Submerged"

## Global Atmosphere
- **Background**: Abyss (#020408).
- **Ambient Elements**: 
  - Static noise layer.
  - `CoralDrifters`: Floating, blurry organic shapes background (z-0).

## Typography
- **Headlines**: Display font, Tight tracking.
- **Hero Title**: "Clarity" uses text-transparent `bg-clip-text` `bg-gradient-to-r from-white to-white/50`.
- **Subheads**: `text-white/60`.
- **Footer**: Massive "Titan" Brand text, `text-[12rem]` to `text-[20rem]`, `font-bold`, `tracking-tighter`, `text-white/[0.02]` or subtle gradient.

## Components

### Buttons
- **Primary (Coral)**: Coral color (e.g., `coral-500` / `#FF6B6B`), glowing effect.
- **Secondary (Glass)**: Glass effect, icon on right.

### Navigation
- **IslandNav**: Floating, pill-shaped, expands on hover.

### Cards (Bento Grid)
- **Style**: "Forged Glass".
- **Features**: Thick borders, inner highlights, glassmorphism.
- **Hover**: Spring physics (svelte/motion).

### Visualization
- **DeepfakeChart**: Holographic glowing line chart or bar graph.
  - Line: `coral-500` with glow.
  - Fill: Gradient fade.

## Interaction & Animation
- **Entry Animations**: `IntersectionObserver` or Svelte `in:fly`.
  - Specs: `y: 30px` -> `0`, `opacity: 0` -> `1`, `duration: 1000ms`, `ease: cubic-out`.
- **Hover Effects**: `svelte/motion` (spring).

## Setup
- **Framework**: SvelteKit + TailwindCSS.
- **Icons**: `lucide-svelte` (1.5px stroke).
