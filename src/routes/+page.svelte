<script lang="ts">
	import heroVideo from "$lib/assets/nuchy2.mp4";

	let innerWidth = $state(1440);
	let innerHeight = $state(900);

	const overlayWord = "NUCHY";
	const overlayColor = "#202020";
	const maskOpacity = 1;
	const maskFontFamily = "'Bevan', serif";
	const strokeWidth = 6;
	const maskFontSize = 10;
	const overlayHeight = 25;

	const viewBoxWidth = $derived(
		(innerWidth / Math.max(innerHeight, 1)) * 100,
	);
	const overlayY = $derived(100 - overlayHeight);
	const wordX = $derived(viewBoxWidth * 0.5);
	const wordOffset = $derived(maskFontSize * 0.6);
	const wordY = $derived(overlayY + wordOffset);
</script>

<svelte:head>
	<title>NUCHY</title>
	<link rel="preconnect" href="https://fonts.googleapis.com" />
	<link
		rel="preconnect"
		href="https://fonts.gstatic.com"
		crossorigin="anonymous"
	/>
	<link
		href="https://fonts.googleapis.com/css2?family=Bevan&display=swap"
		rel="stylesheet"
	/>
</svelte:head>

<svelte:window bind:innerWidth bind:innerHeight />

<section class="hero">
	<video
		class="hero-video"
		autoplay
		muted
		loop
		playsinline
		preload="auto"
		aria-hidden="true"
	>
		<source src={heroVideo} type="video/mp4" />
	</video>

	<svg
		class="hero-overlay"
		viewBox={`0 0 ${viewBoxWidth} 100`}
		aria-hidden="true"
	>
		<defs>
			<mask
				id="hero-overlay-mask"
				maskUnits="userSpaceOnUse"
				maskContentUnits="userSpaceOnUse"
				x="0"
				y="0"
				width={viewBoxWidth}
				height="100"
			>
				<rect
					x="0"
					y="0"
					width={viewBoxWidth}
					height="100"
					fill="white"
				/>
				<text
					x={wordX}
					y={wordY}
					fill="black"
					font-family={maskFontFamily}
					font-size={maskFontSize}
					font-weight="400"
					letter-spacing="0"
					dominant-baseline="middle"
					text-anchor="middle"
				>
					{overlayWord}
				</text>
			</mask>
		</defs>

		<rect
			x="0"
			y={overlayY}
			width={viewBoxWidth}
			height={overlayHeight}
			fill={overlayColor}
			fill-opacity={maskOpacity}
			mask="url(#hero-overlay-mask)"
		/>

		<text
			x={wordX}
			y={wordY}
			fill="none"
			stroke={overlayColor}
			stroke-opacity={maskOpacity}
			stroke-width={strokeWidth}
			stroke-linejoin="round"
			vector-effect="non-scaling-stroke"
			font-family={maskFontFamily}
			font-size={maskFontSize}
			font-weight="400"
			letter-spacing="0"
			dominant-baseline="middle"
			text-anchor="middle"
		>
			{overlayWord}
		</text>
	</svg>
</section>

<style>
	:global(html, body) {
		background: #0c0b09;
	}

	.hero {
		position: relative;
		width: 100vw;
		height: 100vh;
		height: 100svh;
		overflow: hidden;
		background: #0c0b09;
		isolation: isolate;
	}

	.hero-video {
		display: block;
		width: 100%;
		height: 100%;
		object-fit: cover;
	}

	.hero-overlay {
		position: absolute;
		inset: 0;
		width: 100%;
		height: 100%;
		pointer-events: none;
	}
</style>
