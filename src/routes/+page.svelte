<script lang="ts">
	import heroVideo from "$lib/assets/nuchy2.mp4";

	const overlayLetters = ["N", "U", "C", "H", "Y"];
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
		href="https://fonts.googleapis.com/css2?family=Cormorant+Garamond:wght@400;500;600;700&family=Manrope:wght@400;500;600&display=swap"
		rel="stylesheet"
	/>
</svelte:head>

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
		viewBox="0 0 100 100"
		preserveAspectRatio="none"
		aria-hidden="true"
	>
		<defs>
			<mask
				id="hero-overlay-mask"
				maskUnits="userSpaceOnUse"
				maskContentUnits="userSpaceOnUse"
				x="0"
				y="0"
				width="100"
				height="100"
			>
				<rect x="0" y="0" width="100" height="100" fill="white" />
				<text
					x="33.5"
					y="12.5"
					fill="black"
					font-family="'Cormorant Garamond', serif"
					font-size="12.5"
					font-weight="700"
					letter-spacing="0.12em"
					text-anchor="middle"
				>
					{#each overlayLetters as letter, index}
						<tspan x="33.5" dy={index === 0 ? "0" : "0.92em"}
							>{letter}</tspan
						>
					{/each}
				</text>
			</mask>
		</defs>

		<rect
			x="0"
			y="0"
			width="33"
			height="100"
			fill="#171614"
			fill-opacity="0.92"
			mask="url(#hero-overlay-mask)"
		/>
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
