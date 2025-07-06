<script lang="ts">
	import { localeStore } from '../i18n.svelte';
	import { _ } from 'svelte-i18n';
	import Carousel from 'svelte-light-carousel';
	import { onMount } from 'svelte';
	const photos = Array.from({ length: 28 }, (_, i) => ({ src: `/${i + 1}.png`, key: i + 1 }));
	let dotCarousel: HTMLDivElement; // 썸네일 캐러셀 요소를 참조하기 위한 변수
	onMount(() => {
		if (dotCarousel) {
			dotCarousel.scrollTo({ left: 0, behavior: 'auto' });
		}
	});
</script>

<section class="gallery">
	<div class="header">
		<h2 class="title {localeStore.locale}">{$_('gallery.title')}</h2>
	</div>
	<Carousel slides={photos} arrows={true}>
		<div slot="slide" let:slide>
			<img class="thumbnail" src={slide.src} alt="" />
		</div>
		<div slot="dots" let:dots let:scrollTo>
			<!-- 동그라미 인디케이터 -->
			<div class="custom-dots">
				{#each dots as dot, i (i)}
					<span
						class="dot {dot.active ? 'active' : ''}"
						on:click={() => scrollTo(i)}
					></span>
				{/each}
			</div>
			<!-- 기존 썸네일 프리뷰 -->
			<div class="carousel-dots-container">
				<button class="dot-arrow dot-prev-arrow" on:click={() => dotCarousel.scrollBy({ left: -70, behavior: 'smooth' })}>
					<svg width="20" height="20" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
						<path d="M15.41 7.41L14 6L8 12L14 18L15.41 16.59L10.83 12L15.41 7.41Z" fill="currentColor"/>
					</svg>
				</button>
				<div class="carousel-dots" bind:this={dotCarousel}>
					{#each dots as dot, i (i)}
						<button
							class="carousel-dot {dot.active ? 'active' : ''}"
							on:click={() => scrollTo(i)}
						>
							<img src={photos[i].src} alt={`thumbnail ${i + 1}`} class="dot-thumbnail" />
						</button>
					{/each}
				</div>
				<button class="dot-arrow dot-next-arrow" on:click={() => dotCarousel.scrollBy({ left: 70, behavior: 'smooth' })}>
					<svg width="20" height="20" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
						<path d="M8.59 16.59L10 18L16 12L10 6L8.59 7.41L13.17 12L8.59 16.59Z" fill="currentColor"/>
					</svg>
				</button>
			</div>
		</div>
	</Carousel>
</section>

<style lang="scss">
	section.gallery {
		padding: 4.5em 2em 2em 2em;
		background-color: $white;
	}

	.header {
		margin-bottom: 2em;
	}

	h2.title {
		text-align: center;
		&.en {
			@extend .title-font-en;
			letter-spacing: 1px;
		}

		&.kr {
			@extend .title-font-kr;
			letter-spacing: 1px;
		}
	}

	p.sub-title {
		text-align: center;
		&.kr {
			margin-top: 0.9em;
			font-size: 0.9rem;
		}

		&.en {
			margin-top: 0.5em;
			font-size: 1.2rem;
		}
	}

	img.thumbnail {
		display: block;
		max-width: 100vw;
		max-height: 60vh;
		width: auto;
		height: auto;
		object-fit: contain;
		margin: 0 auto;
		border-radius: 4px;
	}

	/* svelte-light-carousel basic styles */
	:global(.carousel) {
		width: 100%;
		height: auto;
		position: relative;
	}

	:global(.carousel-slide) {
		display: inline-flex;
		justify-content: center;
		align-items: center;
		width: auto;
		height: auto;
		position: relative;
	}

	:global(.carousel-slide img) {
		max-width: 100%;
		margin: 50% 0%;
	}

	/* Material UI/Ant Design 스타일의 메인 화살표 */
	:global(.carousel-arrow) {
		position: absolute;
		top: 50%;
		transform: translateY(-50%);
		background-color: rgba(255, 255, 255, 0.9);
		color: #333;
		border: none;
		padding: 12px;
		cursor: pointer;
		z-index: 10;
		border-radius: 50%;
		width: 48px;
		height: 48px;
		display: flex;
		justify-content: center;
		align-items: center;
		box-shadow: 0 2px 8px rgba(0, 0, 0, 0.15);
		transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
		backdrop-filter: blur(10px);
		font-size: 0; /* 텍스트 화살표 숨기기 */
		
		&:hover {
			background-color: rgba(255, 255, 255, 1);
			box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
			transform: translateY(-50%) scale(1.05);
		}
		
		&:active {
			transform: translateY(-50%) scale(0.95);
		}
		
		/* Material Icons 화살표 추가 */
		&::before {
			content: '';
			width: 24px;
			height: 24px;
			background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='24' height='24' viewBox='0 0 24 24' fill='none'%3E%3Cpath d='M15.41 7.41L14 6L8 12L14 18L15.41 16.59L10.83 12L15.41 7.41Z' fill='%23333'/%3E%3C/svg%3E");
			background-size: contain;
			background-repeat: no-repeat;
			background-position: center;
			transition: transform 0.2s ease;
		}
		
		&:hover::before {
			transform: scale(1.1);
		}
	}

	:global(.prev-arrow) {
		left: 16px;
	}

	:global(.next-arrow) {
		right: 16px;
		
		&::before {
			background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='24' height='24' viewBox='0 0 24 24' fill='none'%3E%3Cpath d='M8.59 16.59L10 18L16 12L10 6L8.59 7.41L13.17 12L8.59 16.59Z' fill='%23333'/%3E%3C/svg%3E");
		}
	}

	:global(.carousel-dots) {
		display: flex;
		justify-content: center;
		margin-top: 1em;
		overflow-x: auto;
		padding: 0.5em 0;
		scroll-behavior: smooth;
		-ms-overflow-style: none;
		scrollbar-width: none;
		&::-webkit-scrollbar {
			display: none;
		}
		max-width: 370px;
		white-space: nowrap;
	}

	:global(.carousel-dots-container) {
		display: flex;
		align-items: center;
		justify-content: center;
		position: relative;
		width: 100%;
	}

	:global(.dot-arrow) {
		background-color: rgba(255, 255, 255, 0.8);
		color: #333;
		border: none;
		padding: 8px;
		cursor: pointer;
		z-index: 10;
		border-radius: 50%;
		margin: 0 8px;
		flex-shrink: 0;
		width: 36px;
		height: 36px;
		display: flex;
		justify-content: center;
		align-items: center;
		box-shadow: 0 1px 4px rgba(0, 0, 0, 0.1);
		transition: all 0.2s ease;
		
		&:hover {
			background-color: rgba(255, 255, 255, 1);
			box-shadow: 0 2px 8px rgba(0, 0, 0, 0.15);
			transform: scale(1.05);
		}
		
		&:active {
			transform: scale(0.95);
		}
	}

	:global(.carousel-dot) {
		background: none;
		border: 2px solid transparent;
		padding: 0;
		cursor: pointer;
		margin: 0 5px;
		width: 60px;
		height: 60px;
		border-radius: 4px;
		flex-shrink: 0;
		transition: all 0.2s ease;
		
		&:hover {
			transform: scale(1.05);
		}
	}

	:global(.carousel-dot.active) {
		border-color: $primary-color;
		box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
	}

	:global(.dot-thumbnail) {
		width: 100%;
		height: 100%;
		object-fit: cover;
		display: block;
		border-radius: 2px;
	}

	.custom-dots {
		position: absolute;
		bottom: 20%;
		left: 50%;
		transform: translateX(-50%);
		display: flex;
		justify-content: center;
		align-items: center;
		gap: 0.4em;
		z-index: 2;
	}

	.dot {
		width: 6px;
		height: 6px;
		border-radius: 50%;
		background: rgba(255, 255, 255, 0.6);
		display: inline-block;
		transition: all 0.3s ease;
		cursor: pointer;
		
		&:hover {
			background: rgba(255, 255, 255, 0.8);
			transform: scale(1.2);
		}
	}

	.dot.active {
		background: $primary-color;
		transform: scale(1.3);
		box-shadow: 0 0 8px rgba(0, 0, 0, 0.2);
	}
</style>
