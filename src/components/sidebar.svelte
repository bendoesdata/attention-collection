<script>
    import * as animateScroll from "svelte-scrollto";

	export let open = false
</script>

<aside class="absolute w-full h-full border-r-2 shadow-lg" class:open style="background-color: #2e5b3c; color: #yellow">
	<nav class="p-12 text-xl">
		<div class="block" on:click={() => animateScroll.scrollTo({element: '#article-section', duration: 2000, offset: -600})} on:click={() => open = !open}>ARTICLES</div>
		<div class="block" on:click={() => animateScroll.scrollTo({element: '#book-page', duration: 2000, offset: -800})} on:click={() => open = !open}>BOOKS</div>
	</nav>
</aside>

<style>
	aside {
		left: -100%;
		transition: top 0.3s ease-in-out;
        top: 0
	}
	
	.open {
		left: 0;
        position: sticky;
        height: 100vh
	}

    .block {
        line-height: 1.8;
        font-size: 32px;
        padding-bottom: 20px;
        padding-top: 30px
    }
</style>