<script>
	import '~/styles/app.css';
	import '~/styles/global.scss';
	import Topbar from '~/components/Topbar.svelte';
	import { isLoaded, predictedColor, rootRem } from '~/store';
	import Article from '~/components/article/Article.svelte';
	import { onMount } from 'svelte';
	import { Spinner } from 'flowbite-svelte';

	let topBarHeight = 0;

	let minScreenWidth = 1300;
	let minColumWidth = Math.floor(minScreenWidth / 24) - rootRem * 2;

	onMount(async () => {
		isLoaded.set(true);
	});
</script>

<svelte:head>
	<title>Transformer Explainer</title>
</svelte:head>

<div
	id="app"
	style={`--min-screen-width:${minScreenWidth}px;--min-column-width:${minColumWidth}px;--predicted-color:${predictedColor};`}
>
	<div id="landing">
		<header bind:offsetHeight={topBarHeight}>
			<Topbar />
		</header>
		<main id="main" style={`padding-top:${topBarHeight}px`}>
			{#if $isLoaded}
				<slot />
			{:else}
				<div class="flex h-full w-full items-center justify-center"><Spinner color="purple" /></div>
			{/if}
		</main>
	</div>
	<div class="article h-auto w-full">
		<Article></Article>
	</div>
	<!-- <footer></footer> -->
</div>

<style lang="scss">
	#app {
		height: 100vh;
		min-width: 900px;
	}

	#landing {
		height: 100%;
		width: 100%;
		min-width: var(--min-screen-width);
	}

	header {
		min-width: var(--min-screen-width);
		width: 100%;
		position: fixed;
		z-index: 999;
	}
	main {
		position: relative;
		height: 100%;
		width: 100%;
		display: flex;
		justify-content: start;
		overflow: hidden;
	}
</style>
