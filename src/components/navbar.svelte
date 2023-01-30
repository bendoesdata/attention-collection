<script>
	import Hamburger from './hamburger.svelte'
	import Menu from './menu.svelte'
	
	export let sidebar = false
</script>

<header class="flex justify-between p-2 items-center text-gray-600 border-b-2 sticky" style="background-color: #2e5b3c">
	<nav class="flex">
		<Hamburger bind:open={sidebar}/>
	</nav>
	
	<Menu/>
</header>

<style>
    .sticky {
	position: -webkit-sticky;
	position: sticky;
	top: 0;
	z-index: 100;
	color: yellow;
	background-color: #2e5b3c;
}
</style>