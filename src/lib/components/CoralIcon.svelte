<script>
	let { size = 24, class: className = '' } = $props();
</script>

<svg
	width={size}
	height={size}
	viewBox="0 0 32 32"
	fill="none"
	xmlns="http://www.w3.org/2000/svg"
	class={className}
>
	<defs>
		<linearGradient
			id="coral-branch-grad"
			x1="4"
			y1="4"
			x2="28"
			y2="28"
			gradientUnits="userSpaceOnUse"
		>
			<stop stop-color="#FF9F7D" />
			<stop offset="1" stop-color="#E04F30" />
		</linearGradient>
	</defs>

	<path
		fill-rule="evenodd"
		clip-rule="evenodd"
		d="M16 30C18.5 30 19.5 28 19.5 26C19.5 24 18.5 22 18 20C21 21 24 20 26 18C28.5 15.5 28 12 27.5 11C27 10 25 10 24.5 11C24 12 24.5 14 23 15.5C22 16.5 20.5 17 19 17C20.5 15 22 12.5 22 10C22 7 20 4 19 3C18 2 16 3 16.5 4.5C17 6 18 8 17.5 10C17 12 15 14 14 15C14 13 15 10 14.5 8C14 6 12 5 11.5 6C11 7 11.5 9.5 11.5 11.5C11.5 13.5 10 15 8.5 14C7 13 6.5 10 6 10C5.5 10 3.5 11 3.5 12C3.5 13 4 16 6.5 18.5C8.5 20.5 12 20 14 20C13.5 22 12.5 24 12.5 26C12.5 28 13.5 30 16 30Z"
		fill="url(#coral-branch-grad)"
	/>
</svg>
