<script lang="ts">
	import { _ } from 'svelte-i18n';
	import { localeStore } from '../i18n.svelte';

	const letterBottom = "/letter-bottom.webp";
	const letterDeco = "/letter-deco.svg";

	const nameDivider = `
    <svg width="3" height="3" viewBox="0 0 3 3" fill="none" xmlns="http://www.w3.org/2000/svg">
        <circle cx="1.5" cy="1.5" r="1.5" fill="#B99493"/>
    </svg>`;
</script>

<section class="letter">
	<div class="header">
		<img class="header-deco" src={letterDeco} alt="letter header deco" loading="lazy" />
		<h2 class="title {localeStore.locale}">{$_('letter.date')}</h2>
		<p class="sub-title {localeStore.locale}">{$_('letter.sub_title')}</p>
	</div>

	<div class="letter-container">
		<p class="letter {localeStore.locale}">{$_('letter.letter_content')}</p>

		{#if localeStore.isEn}
			<div class="letter-signature">
				<p class="en">with love</p>
				<p class="en">Yungeun & Sujin</p>
			</div>
		{:else}
			<div class="family-description kr">
				<span class="groom-line">김성훈 <span class="name-divider">{@html nameDivider}</span> 윤경숙 &nbsp;의 장남&nbsp; <span class="groom-name">윤근</span></span>
				<span class="bride-line">정재철 <span class="name-divider">{@html nameDivider}</span> 김선순 &nbsp;의 장녀&nbsp; <span class="bride-name">수진</span></span>
			</div>
		{/if}
	</div>
</section>

<img class="letter-bottom" src={letterBottom} alt="letter bottom" loading="lazy" />

<style lang="scss">
	section.letter {
		padding: 4.5em 3.5em 3em 3.5em;
	}

	.header {
		display: flex;
		flex-direction: column;
		align-items: center;
		margin-bottom: 2em;
	}

	img.header-deco {
		width: 12em;
		margin-bottom: 0.8em;
	}

	.title {
		color: $primary-color;
		letter-spacing: 1px;

		&.kr {
			@extend .title-font-kr;
			margin-bottom: 0.4em;
		}

		&.en {
			@extend .title-font-en;
			margin-bottom: 0.2em;
		}
	}

	.sub-title {
		color: $primary-color;
		&.kr {
			font-weight: 500;
			font-size: 1rem;
		}

		&.en {
			font-size: 1.3rem;
			font-weight: 600;
		}
	}

	p.letter {
		text-align: center;
		&.kr {
			line-height: 2.2em;
			font-size: 1rem;
		}
		&.en {
			line-height: 1.8em;
			font-size: 1.2rem;
		}
	}

	.letter-signature {
		margin-top: 2em;

		p {
			text-align: right;
			font-size: 1.2rem;
		}
	}
	img.letter-bottom{
		max-width: 600px;
		width: 100%;
		margin: 0 auto;
	}

	.family-description {
		margin-top: 2em;
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		gap: 0.5em;

		span.groom, span.bride {
			color: $font-color-default;
			font-size: 1.1em;
			text-align: center;
		}

		.name-divider {
			display: flex;
			align-items: center;
			justify-content: center;
			margin: 0 0.5em;
		}

		.groom-line, .bride-line {
			display: flex;
			flex-direction: row;
			align-items: center;
			justify-content: center;
			margin-bottom: 0.5em;
			gap: 0.2em;
		}

		.groom-name, .bride-name {
			font-size: 1.1em;
			font-weight: bold;
			color: $primary-color;
		}
	}
</style>
