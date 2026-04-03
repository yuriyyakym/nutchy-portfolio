<script lang="ts">
	import heroVideo from "$lib/assets/nuchy2.mp4";

	let innerWidth = $state(1440);
	let innerHeight = $state(900);

	const overlayLetters = ["N", "U", "C", "H", "Y"];
	const overlayColor = "#0a0a0a";
	const maskOpacity = 0.95;
	const maskFontFamily = "'Bevan', serif";
	const strokeWidth = 0.2;
	const maskFontSize = 13.8;
	const maskStartY = 23.8;
	const maskLetterStep = 15.6;

	const viewBoxWidth = $derived(
		(innerWidth / Math.max(innerHeight, 1)) * 100,
	);
	const overlayWidth = $derived(viewBoxWidth * 0.5);
	const lettersOffset = $derived(maskFontSize * -0.05);
	const lettersX = $derived(overlayWidth + lettersOffset);
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
					x={lettersX}
					fill="black"
					font-family={maskFontFamily}
					font-size={maskFontSize}
					font-weight="400"
					letter-spacing="0"
					text-anchor="middle"
				>
					{#each overlayLetters as letter, index}
						<tspan
							x={lettersX}
							y={maskStartY + index * maskLetterStep}
							>{letter}</tspan
						>
					{/each}
				</text>
			</mask>
		</defs>

		<rect
			x="0"
			y="0"
			width={overlayWidth}
			height="100"
			fill={overlayColor}
			fill-opacity={maskOpacity}
			mask="url(#hero-overlay-mask)"
		/>

		<text
			x={lettersX}
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
			text-anchor="middle"
		>
			{#each overlayLetters as letter, index}
				<tspan x={lettersX} y={maskStartY + index * maskLetterStep}
					>{letter}</tspan
				>
			{/each}
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
