<script>
	import { slide } from "svelte/transition";
	export let entry
    export let title
	let isOpen = false
	const toggle = () => isOpen = !isOpen

    console.log(entry)
</script>
<style>
	button {border: none; text-transform: uppercase; background: none;display:inline-block; color: inherit; font-size: 14px; cursor: pointer; margin: 0; padding-bottom: 0.5em; padding-top: 0.5em}

	svg { transition: transform 0.2s ease-in; display: inline-block;
	}

    .border-div {
        border: 1px solid #f3f3f3; 
        padding: 20px;
        max-width: 700px;
        font-style: italic;
    }

    .border-div li {
        margin-bottom: 20px;
    }
	
	[aria-expanded=true] svg { transform: rotate(0.25turn); }
</style>
<button on:click={toggle} aria-expanded={isOpen}><svg style="tran"  width="10" height="10" fill="none" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" viewBox="0 0 24 24" stroke="currentColor"><path d="M9 5l7 7-7 7"></path></svg> {title} </button>
{#if isOpen}
<div class="border-div">
<ul transition:slide={{ duration: 300 }}>
	{#each entry as item}
	<li>— {item.text}</li>
	{/each}
</ul>
</div>
{/if}
