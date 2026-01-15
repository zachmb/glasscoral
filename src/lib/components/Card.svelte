<script lang="ts">
	interface Props {
		children: import('svelte').Snippet;
		class?: string;
	}

	let { children, class: className = '' }: Props = $props();
</script>

<div class="glass-panel p-6 relative overflow-hidden group {className}">
	<!-- Subtle Inner Lighting Effect -->
	<div class="absolute inset-0 bg-gradient-to-br from-white/5 to-transparent opacity-0 pointer-events-none transition-opacity duration-500 group-hover:opacity-100"></div>
	
	<!-- Content -->
	<div class="relative z-10">
		{@render children()}
	</div>
</div>
