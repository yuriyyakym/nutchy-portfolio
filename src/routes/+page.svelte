<script lang="ts">
	import heroVideo from "$lib/assets/nuchy2.mp4";
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
	<video class="hero-video" autoplay muted loop playsinline preload="auto">
		<source src={heroVideo} type="video/mp4" />
	</video>

	<div class="hero-wash" aria-hidden="true"></div>
	<div class="hero-grid" aria-hidden="true"></div>

	<div class="hero-copy">
		<p class="eyebrow">Visual direction / moving image / portrait</p>
		<h1>NUCHY</h1>
		<p class="tagline">
			A full-screen intro pass built around the new video.
		</p>
	</div>

	<div class="hero-note">
		<span>Header study</span>
		<span>Masking next</span>
	</div>
</section>

<style>
	:global(html) {
		background: #0c0b09;
	}

	:global(body) {
		margin: 0;
		background: radial-gradient(
				circle at top,
				rgba(188, 141, 76, 0.12),
				transparent 34%
			),
			#0c0b09;
		color: #f8f3ea;
		font-family: "Manrope", sans-serif;
	}

	:global(*) {
		box-sizing: border-box;
	}

	.hero {
		--frame-padding: clamp(1.2rem, 2vw, 2rem);
		position: relative;
		min-height: 100vh;
		min-height: 100svh;
		display: grid;
		align-items: end;
		padding: var(--frame-padding);
		overflow: clip;
		isolation: isolate;
		background: #120f0b;
	}

	.hero::after {
		content: "";
		position: absolute;
		inset: var(--frame-padding);
		border: 1px solid rgba(255, 248, 236, 0.14);
		pointer-events: none;
		z-index: 3;
	}

	.hero-video {
		position: absolute;
		inset: 0;
		width: 100%;
		height: 100%;
		object-fit: cover;
		filter: saturate(0.9) contrast(1.05) brightness(0.82);
	}

	.hero-wash {
		position: absolute;
		inset: 0;
		background: linear-gradient(
				180deg,
				rgba(11, 10, 8, 0.18) 0%,
				rgba(11, 10, 8, 0.1) 28%,
				rgba(11, 10, 8, 0.52) 100%
			),
			linear-gradient(
				90deg,
				rgba(11, 10, 8, 0.58) 0%,
				rgba(11, 10, 8, 0.14) 42%,
				rgba(11, 10, 8, 0.36) 100%
			);
		z-index: 1;
	}

	.hero-grid {
		position: absolute;
		inset: 0;
		background-image: linear-gradient(
				rgba(255, 248, 236, 0.05) 1px,
				transparent 1px
			),
			linear-gradient(
				90deg,
				rgba(255, 248, 236, 0.05) 1px,
				transparent 1px
			);
		background-size:
			100% 100%,
			100% 100%,
			64px 64px,
			64px 64px;
		mask-image: linear-gradient(
			180deg,
			transparent 0%,
			rgba(0, 0, 0, 0.25) 25%,
			rgba(0, 0, 0, 0.7) 100%
		);
		opacity: 0.35;
		z-index: 2;
	}

	.hero-copy,
	.hero-note {
		position: relative;
		z-index: 4;
	}

	.hero-copy {
		display: grid;
		gap: 0.9rem;
		max-width: min(42rem, 78vw);
		padding: clamp(1rem, 1.8vw, 1.5rem);
		margin-bottom: clamp(1.4rem, 3vw, 2.8rem);
		backdrop-filter: blur(10px);
		background: linear-gradient(
			135deg,
			rgba(13, 11, 9, 0.44),
			rgba(13, 11, 9, 0.18)
		);
		border: 1px solid rgba(255, 248, 236, 0.1);
	}

	.eyebrow,
	.tagline,
	.hero-note {
		letter-spacing: 0.18em;
		text-transform: uppercase;
	}

	.eyebrow {
		margin: 0;
		font-size: 0.72rem;
		color: rgba(248, 243, 234, 0.76);
	}

	h1 {
		margin: 0;
		font-family: "Cormorant Garamond", serif;
		font-size: clamp(4.8rem, 15vw, 11rem);
		line-height: 0.9;
		font-weight: 600;
		letter-spacing: 0.08em;
		text-transform: uppercase;
		text-wrap: balance;
	}

	.tagline {
		margin: 0;
		font-size: 0.78rem;
		color: rgba(248, 243, 234, 0.72);
	}

	.hero-note {
		position: absolute;
		left: calc(var(--frame-padding) * 2);
		right: calc(var(--frame-padding) * 2);
		bottom: calc(var(--frame-padding) * 2);
		display: flex;
		justify-content: space-between;
		gap: 1rem;
		font-size: 0.65rem;
		color: rgba(248, 243, 234, 0.68);
	}

	@media (max-width: 720px) {
		.hero {
			align-items: end;
		}

		.hero-copy {
			max-width: none;
			margin-bottom: 4.8rem;
		}

		.hero-note {
			left: var(--frame-padding);
			right: var(--frame-padding);
			bottom: 1.8rem;
			flex-direction: column;
			align-items: flex-start;
		}
	}
</style>
