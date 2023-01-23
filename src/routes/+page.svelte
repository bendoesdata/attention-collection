<script>
import Bookshelf from "../components/bookshelf.svelte"
import Reading from "../components/reading.svelte"
import Accordion from '../components/accordion.svelte'

const description = [
    {"text": "An Attention Collection is a term coined by writer David Dark that captures all the little things that we collect along our way—stories, songs, books, movies, ideas. It can be anything really, as long as you saw the thing and you remembered it. This page is an ongoing experiment to automate my own personal attention collection. All the articles, books, movies, and more that I come across. Instead of relying on my memory, which is fickle, I've setup a few trackers online to automatically store the things I read/watch/listen to in order to pull them all into one place: this web page."}
]

</script>
<main id="app">
    <h1>Attention Collection</h1>
    <Accordion entry={description} title="what is this?" />
    <div class="row">
        <div class="col-1">
            <Reading />
        </div>
        <div class="col-2">
            <Bookshelf />
        </div>
    </div>
</main>

<style>
    @import url('https://fonts.googleapis.com/css2?family=Figtree:wght@300;700;900&display=swap');

    #app {
        /* olive green */
        color: #9ca243; 
        /* black */
        background-color: #0d0314;
        font-family: "Figtree", sans-serif;
        padding: 5px
    }

    h1 {
        padding: 10px;
        font-weight: 900;
        font-size: 3rem
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
