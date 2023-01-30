<script>
import Bookshelf from "../components/bookshelf.svelte"
import CurrentBooks from "../components/currentBooks.svelte"
import Reading from "../components/reading.svelte"
import Accordion from '../components/accordion.svelte'
import Navbar from '../components/navbar.svelte'
import Sidebar from '../components/sidebar.svelte'

let open = false

const description = [
    {"text": "An Attention Collection is a term coined by writer David Dark that captures all the little things that we collect along our way—stories, songs, books, movies, ideas. It can be anything really, as long as you saw the thing and you remembered it. This page is an ongoing experiment to automate my own personal attention collection. All the articles, books, movies, and more that I come across. Instead of relying on my memory, which is fickle, I've setup a few trackers online to automatically store the things I read/watch/listen to in order to pull them all into one place: this web page."}
]

</script>
<main id="app">
    <Navbar bind:sidebar={open}/>
    <Sidebar bind:open/>
    <h1>Attention Collection</h1>
    <Accordion entry={description} title="what is this?" />
    <br>
    <br>
    <div class="row inverse-background-color">
        <h2 class="title-section">CURRENT</h2>
        <CurrentBooks />
        <div class="col-1">
        </div>
        <div class="col-2">

        </div>
    </div>
    <br>
    <br>
    <div class="row">
        <h2 class="title-section">PAST</h2>
        <div class="col-1" style="margin-top: 80px">
            <h2>Articles</h2>
            <Reading id="articles" />
        </div>
        <div class="col-2" style="margin-top: 80px">
            <h2>Books</h2>
            <Bookshelf id="books" />
        </div>
    </div>
</main>

<!-- add Tailwind so the hambyrger menu works -->
<svelte:head>
	<link href="https://unpkg.com/tailwindcss@^1.0/dist/tailwind.min.css" rel="stylesheet"/>
</svelte:head>

<style>
    @import url('https://fonts.googleapis.com/css2?family=Figtree:wght@300;700;900&display=swap');
    #app {
        font-family: "Figtree", sans-serif;
        padding: 5px
    }

    h1 {
        padding: 10px;
        font-weight: 900;
        font-size: 3rem
    }

    h2 {
        font-size: 2.5rem;
    }

    .title-section {
        padding: 10px;
    }

    a { color: inherit; } 

    .row {
        display: flex;
        margin: 0 auto;
        padding: 10px;
    }
    .col-1 {
        flex: 1;
        padding: 5px;
    }
    .col-2 {
        flex: 1;
        padding: 5px
    }
    @media screen and (max-width: 800px) {
        .row {
            display: block;
        }
    }
</style>
