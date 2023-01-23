<script>
import Bookshelf from "../components/bookshelf.svelte"
import Reading from "../components/reading.svelte"
</script>
<main id="app">
    <h1>Attention Collection</h1>
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
    }

    h1 {
        padding: 10px
    }

    a { color: inherit; } 

    .row {
        display: flex;
        margin: 0 auto;
        padding: 10px;
    }
    .col-1 {
        flex: 1;
    }
    .col-2 {
        flex: 1;
    }
    @media screen and (max-width: 800px) {
        .row {
            display: block;
        }
    }
</style>
