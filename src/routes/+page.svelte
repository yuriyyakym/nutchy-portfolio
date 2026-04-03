<script lang="ts">
	import heroVideo from "$lib/assets/nuchy2.mp4";

	type GalleryImage = {
		src: string;
		alt: string;
		label: string;
		code: string;
	};

	const imageModules = import.meta.glob(
		"/src/lib/assets/pictures/*.{avif,jpg,jpeg,png,webp}",
		{
			eager: true,
			import: "default",
		},
	) as Record<string, string>;

	const galleryImages: GalleryImage[] = Object.entries(imageModules)
		.sort(([left], [right]) => left.localeCompare(right))
		.map(([path, src], index) => {
			const code =
				path
					.split("/")
					.pop()
					?.replace(/\.[^.]+$/, "") ?? `still-${index + 1}`;

			return {
				src,
				code,
				alt: `Nuchy portfolio still ${String(index + 1).padStart(2, "0")}`,
				label: String(index + 1).padStart(2, "0"),
			};
		});

	let scrollY = $state(0);
	let innerHeight = $state(0);
	let copyHeight = $state(0);

	const clamp = (value: number, min: number, max: number) =>
		Math.min(Math.max(value, min), max);

	const heroRange = $derived(Math.max(innerHeight, 1) * 1.2);
	const heroScroll = $derived(clamp(scrollY, 0, heroRange));
	const heroProgress = $derived(
		clamp(heroScroll / Math.max(innerHeight, 1), 0, 1),
	);
	const videoShift = $derived(Math.round(heroScroll * 0.16));
	const washShift = $derived(Math.round(heroScroll * 0.08));
	const gridShift = $derived(Math.round(heroScroll * 0.05));
	const blurShift = $derived(Math.round(heroScroll * 0.22));
	const copyShift = $derived(Math.round(heroScroll * -0.5));
	const noteShift = $derived(Math.round(heroScroll * 0.06));
	const copyStart = $derived(
		Math.round(clamp(Math.max(innerHeight - copyHeight - 72, 0), 180, 520)),
	);
	const copyTop = $derived(Math.max(0, copyStart + copyShift));
	const copyStickProgress = $derived(
		clamp(1 - copyTop / Math.max(copyStart, 1), 0, 1),
	);
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

<svelte:window bind:scrollY bind:innerHeight />

<div
	class="page-flow"
	style={`--copy-top: ${copyTop}px; --copy-stick-progress: ${copyStickProgress}; --hero-progress: ${heroProgress};`}
>
	<div class="hero-copy" bind:clientHeight={copyHeight}>
		<p class="eyebrow">Visual direction / moving image / portrait</p>
		<h1>NUCHY</h1>
		<p class="tagline">
			A full-screen intro pass built around the new video.
		</p>
	</div>

	<section
		class="hero"
		style={`--hero-progress: ${heroProgress}; --video-shift: ${videoShift}px; --wash-shift: ${washShift}px; --grid-shift: ${gridShift}px; --blur-shift: ${blurShift}px; --note-shift: ${noteShift}px;`}
	>
		<video
			class="hero-video"
			autoplay
			muted
			loop
			playsinline
			preload="auto"
		>
			<source src={heroVideo} type="video/mp4" />
		</video>

		<div class="hero-wash" aria-hidden="true"></div>
		<div class="hero-grid" aria-hidden="true"></div>
		<div class="hero-blur-title" aria-hidden="true">
			<span>NUCHY</span>
		</div>

		<div class="hero-note">
			<span>Header study</span>
			<span>Scroll for stills</span>
		</div>
	</section>

	<section class="gallery-section" aria-labelledby="gallery-title">
		<div class="gallery-shell">
			<div class="gallery-intro">
				<p class="gallery-kicker">Selected stills</p>
				<h2 id="gallery-title">
					A gallery block under the intro so the page finally moves.
				</h2>
				<p class="gallery-copy">
					This is a clean first pass for scrolling. The video header
					stays dominant, then the pictures take over below without
					competing with the upcoming mask treatment.
				</p>
			</div>

			<div class="gallery-masonry">
				{#each galleryImages as image, index (image.src)}
					<figure
						class="gallery-card"
						class:featured={index % 5 === 0}
					>
						<img
							src={image.src}
							alt={image.alt}
							loading={index < 4 ? "eager" : "lazy"}
							decoding="async"
						/>
						<figcaption>
							<span>{image.label}</span>
							<span>{image.code}</span>
						</figcaption>
					</figure>
				{/each}
			</div>
		</div>
	</section>
</div>

<style>
	:global(html) {
		background: #0c0b09;
	}

	:global(body) {
		margin: 0;
		overflow-x: clip;
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

	.page-flow {
		--frame-padding: clamp(1.2rem, 2vw, 2rem);
		display: grid;
		grid-template-areas:
			"stage"
			"gallery";
		align-items: start;
	}

	.hero {
		grid-area: stage;
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
		transform: translate3d(0, var(--video-shift), 0)
			scale(calc(1.08 + var(--hero-progress) * 0.08));
		transform-origin: center center;
		will-change: transform;
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
		transform: translate3d(0, var(--wash-shift), 0);
		will-change: transform;
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
		transform: translate3d(0, var(--grid-shift), 0);
		will-change: transform;
	}

	.hero-blur-title {
		position: absolute;
		left: clamp(1.4rem, 4vw, 3.8rem);
		right: clamp(1.4rem, 4vw, 3.8rem);
		bottom: clamp(6rem, 16vh, 10rem);
		z-index: 2;
		pointer-events: none;
		transform: translate3d(0, var(--blur-shift), 0)
			scale(calc(1.02 + var(--hero-progress) * 0.08));
		transform-origin: left bottom;
		opacity: calc(0.22 + var(--hero-progress) * 0.14);
		will-change: transform, opacity;
	}

	.hero-blur-title span {
		display: block;
		font-family: "Cormorant Garamond", serif;
		font-size: clamp(6.5rem, 24vw, 18rem);
		line-height: 0.78;
		font-weight: 600;
		letter-spacing: 0.14em;
		text-transform: uppercase;
		color: rgba(248, 243, 234, 0.3);
		filter: blur(18px);
		text-shadow: 0 0 3rem rgba(201, 159, 103, 0.18);
	}

	.hero-copy {
		grid-area: stage;
		position: sticky;
		top: 0;
		align-self: start;
		z-index: 20;
		display: grid;
		gap: 0.9rem;
		width: min(42rem, calc(100vw - (var(--frame-padding) * 2)));
		max-width: min(42rem, 78vw);
		padding: clamp(1rem, 1.8vw, 1.5rem);
		margin-top: var(--copy-top);
		margin-left: var(--frame-padding);
		overflow: hidden;
		backdrop-filter: blur(10px);
		background: linear-gradient(
			135deg,
			rgba(13, 11, 9, 0.44),
			rgba(13, 11, 9, 0.18)
		);
		border: 1px solid rgba(255, 248, 236, 0.1);
		box-shadow: 0 1.8rem 4rem rgba(0, 0, 0, 0.24);
		transform: scale(calc(1 - var(--copy-stick-progress) * 0.04));
		transform-origin: left top;
		opacity: calc(1 - var(--hero-progress) * 0.12);
		will-change: margin-top, transform, opacity;
	}

	.hero-copy::before {
		content: "";
		position: absolute;
		inset: 0;
		background: linear-gradient(
			120deg,
			rgba(255, 248, 236, 0.12) 0%,
			rgba(255, 248, 236, 0.04) 24%,
			transparent 52%
		);
		opacity: calc(0.6 - var(--hero-progress) * 0.2);
		pointer-events: none;
	}

	.hero-copy > * {
		position: relative;
		z-index: 1;
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
		transform: translate3d(0, var(--note-shift), 0);
		opacity: calc(0.92 - var(--hero-progress) * 0.22);
		will-change: transform, opacity;
		z-index: 4;
	}

	@media (max-width: 720px) {
		.hero {
			align-items: end;
		}

		.hero-blur-title {
			bottom: 8rem;
		}

		.hero-copy {
			width: calc(100vw - (var(--frame-padding) * 2));
			max-width: none;
		}

		.hero-note {
			left: var(--frame-padding);
			right: var(--frame-padding);
			bottom: 1.8rem;
			flex-direction: column;
			align-items: flex-start;
		}
	}

	.gallery-section {
		grid-area: gallery;
		position: relative;
		padding: clamp(5rem, 8vw, 8rem) clamp(1.2rem, 2vw, 2rem)
			clamp(6rem, 10vw, 9rem);
		background: linear-gradient(
				180deg,
				rgba(12, 11, 9, 0) 0,
				rgba(12, 11, 9, 0.94) 12rem
			),
			linear-gradient(180deg, #0c0b09 0%, #090806 100%);
	}

	.gallery-section::before {
		content: "";
		position: absolute;
		top: 0;
		left: 0;
		right: 0;
		height: 1px;
		background: linear-gradient(
			90deg,
			transparent 0%,
			rgba(248, 243, 234, 0.18) 20%,
			rgba(248, 243, 234, 0.18) 80%,
			transparent 100%
		);
	}

	.gallery-shell {
		max-width: 1440px;
		margin: 0 auto;
		display: grid;
		grid-template-columns: minmax(16rem, 22rem) minmax(0, 1fr);
		gap: clamp(2rem, 5vw, 4.5rem);
		align-items: start;
	}

	.gallery-intro {
		position: sticky;
		top: 1.5rem;
		display: grid;
		gap: 1rem;
		padding-top: 0.4rem;
	}

	.gallery-kicker,
	.gallery-copy,
	figcaption {
		letter-spacing: 0.14em;
		text-transform: uppercase;
	}

	.gallery-kicker {
		margin: 0;
		font-size: 0.72rem;
		color: rgba(248, 243, 234, 0.78);
	}

	h2 {
		margin: 0;
		font-family: "Cormorant Garamond", serif;
		font-size: clamp(2.5rem, 5vw, 4.5rem);
		line-height: 0.94;
		font-weight: 600;
		max-width: 10ch;
	}

	.gallery-copy {
		margin: 0;
		font-size: 0.72rem;
		line-height: 1.9;
		color: rgba(248, 243, 234, 0.6);
		max-width: 28rem;
	}

	.gallery-masonry {
		columns: 3 15rem;
		column-gap: clamp(1rem, 2vw, 1.5rem);
	}

	.gallery-card {
		break-inside: avoid;
		margin: 0 0 clamp(1rem, 2vw, 1.5rem);
		padding: 0.7rem;
		background: linear-gradient(
			180deg,
			rgba(24, 19, 15, 0.96) 0%,
			rgba(16, 13, 10, 0.96) 100%
		);
		border: 1px solid rgba(248, 243, 234, 0.08);
		box-shadow: 0 20px 55px rgba(0, 0, 0, 0.22);
	}

	.gallery-card.featured {
		background: linear-gradient(
			180deg,
			rgba(43, 31, 20, 0.96) 0%,
			rgba(18, 14, 10, 0.96) 100%
		);
		border-color: rgba(199, 154, 90, 0.28);
	}

	.gallery-card img {
		display: block;
		width: 100%;
		height: auto;
		background: #120f0b;
	}

	figcaption {
		display: flex;
		justify-content: space-between;
		gap: 1rem;
		padding-top: 0.8rem;
		font-size: 0.62rem;
		color: rgba(248, 243, 234, 0.6);
	}

	@media (max-width: 980px) {
		.gallery-shell {
			grid-template-columns: 1fr;
		}

		.gallery-intro {
			position: static;
			max-width: 40rem;
		}

		h2 {
			max-width: 12ch;
		}
	}

	@media (max-width: 720px) {
		.gallery-section {
			padding-top: 4.5rem;
		}

		.gallery-masonry {
			columns: 1;
		}

		h2 {
			max-width: none;
		}
	}

	@media (prefers-reduced-motion: reduce) {
		.hero-video,
		.hero-wash,
		.hero-grid,
		.hero-blur-title,
		.hero-copy,
		.hero-note {
			transform: none;
			opacity: 1;
			transition: none;
		}
	}
</style>
