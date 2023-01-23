<script>
    import { fade } from "svelte/transition";
    import { onMount } from "svelte";
    // store URLs as variables for future reference
    const pastReadUrl =
        "https://openlibrary.org/people/bendextercooley/books/already-read.json";
    const currentlyReadingUrl =
        "https://openlibrary.org/people/bendextercooley/books/currently-reading.json";
    const coverUrl = "http://covers.openlibrary.org/b/olid/";
    const olBookUrl = "https://openlibrary.org/books/";
    // create some empty variables for storing data later
    // these need to be before onMount so they are global, and can be used in the template
    let formattedBooks = [];
    let formattedReading = [];
    let bookList = [];
    let currentList = [];
    let finalBooks, nowReading, booksLength;
    onMount(async () => {
        currentList = await fetch(currentlyReadingUrl).then((x) => x.json());
        bookList = await fetch(pastReadUrl).then((x) => x.json());
        // console.log("data is ready: ", currentList);
        nowReading = formatData(currentList, formattedReading);
        finalBooks = formatData(bookList, formattedBooks);
        booksLength = finalBooks.length;
        console.log(nowReading);
    });
    // function to pull out the data that I want
    function formatData(dataObject, arr) {
        const colors = ["#730A16", "#BB4F24", "#2F7894", "#D28F33"];
        let widths = [60, 70, 90];
        // NOTE: datestamp on OpenLibrary is wrong for now :(
        // sort by date
        //       const dataset = formattedBooks.sort(function(a,b){
        //   return b.read_timestamp - a.read_timestamp;
        // });
        function getRandomInt(max) {
            return Math.floor(Math.random() * max);
        }
        // pull out relevant data
        dataObject.reading_log_entries.forEach((d) => {
            let obj = {};
            if (d.logged_edition == null) {
                obj.ol_id = d.work.key.split("/")[2];
            } else {
                obj.ol_id = d.logged_edition.split("/")[2];
            }
            (obj.title = d.work.title),
            (obj.color = colors[getRandomInt(4)]),
            (obj.width = widths[getRandomInt(3)]),
            (obj.tilt = getRandomInt(4) * 1.3),
            (obj.read_date = d.logged_date),
            (obj.read_timestamp = new Date(d.logged_date.split(",")[0])),
            (obj.book_link = d.work.key),
            (obj.cover_url =
                coverUrl + d.work.cover_edition_key + "-M.jpg"),
            (obj.author = d.work.author_names[0]),
            (obj.author_link = d.work.author_keys[0]);
            arr.push(obj);
        });
        return arr;
    }
    // when user clicks book, display info on left side
    let bookOffShelf;
    function pullOffShelf(data) {
        bookOffShelf = data;
    }
    // switch from bookshelf/list view
    let shelfView = false;
    function toggleView() {
        shelfView = !shelfView;
    }
</script>

<svelte:head>
    <title>Bookshelf</title>
</svelte:head>

<div id="book-page">
    <div>
        <div>
                <h2 style="padding-bottom: 20px">Books</h2>
                <h3>Current</h3>
                <p>On my coffee table or bedside table. You can find all the books I remember reading on my bookshelf below, which pulls live data from the <a href="https://openlibrary.org/" target="_blank">OpenLibrary</a> API. Click on a book to view it on OpenLibrary. Read about why I made this library <a href="/notepad/my-digital-bookshelf">here</a>.</p>
            <div style="min-height: 200px; margin-top: 20px">
                {#if nowReading != null}
                    {#each nowReading as book}
                        <div in:fade
                            style="display: inline-block; margin-left: 10px; margin-right: 10px"
                        >
                            <a
                            href="{olBookUrl}{book.ol_id}"
                            target="_blank">
                                <img 
                                    style="margin-bottom: 10px"
                                    width="100"
                                    src={book.cover_url}
                                    alt={book.title}
                                />
                            </a>
                        </div>
                    {/each}
                    {:else}
                    <div>
                        <p style="text-align: center; margin-bottom: 20px">Getting latest books...</p>
                        <div class="loader" style="margin: 0 auto"></div>
                    </div>
                {/if}
            </div>
        </div>
    </div>

    <div in:fade class="row">
        <div>
            {#if finalBooks != null}
                <!-- {#if shelfView}
                    <button class="toggle-btn" on:click={toggleView}>Show as 📋 list</button>
                {:else}
                    <button class="toggle-btn" on:click={toggleView}>Show as 📚 bookshelf</button>
                {/if} -->
                <h3>Past read</h3>

                <span style="float: right; font-size: 20px">Total books read: <strong>{booksLength}</strong></span>
            {/if}
            <div id="container" style="width: 100%">
                {#if finalBooks != null}
                    {#if shelfView == true}
                        {#each finalBooks as book}
                            <div
                                class="book"
                                on:click={() => pullOffShelf(book)}
                                in:fade="{{ duration: 2000 }}"
                                style="background-color: {book.color}; transform: rotate({book.tilt}deg); width: {book.width}px"
                            >
                                <div class="spine">
                                    <p class="inner rotate">{book.title}</p>
                                </div>
                            </div>
                            {/each}
                        {:else}
                            {#each finalBooks as book}
                                <div class="row book-row" style="max-width: 450px" 
                                on:click={() => pullOffShelf(book)}
                                in:fade="{{ duration: 2000 }}">
                                    <div>
                                            <img 
                                        style="margin-bottom: 10px; width: 100px"
                                        src={book.cover_url}
                                        alt={book.title}
                                    />
                                    </div>
                                </div>
                            {/each}

                        {/if}
                {:else}
                    <div>
                        <p style="margin-left: 20px" id="loading">Organizing books on the shelf...</p>
                    </div>
                {/if}
            </div>
        </div>
    </div>
</div>

<style>
    h1 {
        color: black;
    }
    h2 {
        font-size: 2rem;
    }
    
    a { color: inherit; } 
    .book-row {
        border-bottom: 1px solid gray;
        margin-bottom: 10px
    }
    .row {
        display: flex;
        margin: 0 auto;
        padding: 10px;
    }
    .col-1 {
        flex: 1;
        width: 300px;
    }
    .col-2 {
        flex: 2;
    }
    #container {
        display: flex;
        flex-wrap: wrap;
        padding: 5px;
        margin: 20px;
        min-height: 500px;
    }
    .book {
        height: 150px;
        width: 60px;
        position: relative;
        display: inline-block;
        margin: 3px;
    }
    .inner {
        position: absolute;
        top: 50%;
        left: 40%;
        margin: 10px;
        width: 30px;
        font-size: 20px;
        color: white;
        font-weight: 600;
        text-align: center;
    }
    .loader {
        border: 16px solid #f3f3f3;
        border-radius: 50%;
        border-top: 16px solid #9ca243; 
        width: 60px;
        height:60px;
        -webkit-animation: spin 2s linear infinite; /* Safari */
        animation: spin 2s linear infinite;
    }
    /* Safari */
    @-webkit-keyframes spin {
        0% {
            -webkit-transform: rotate(0deg);
        }
        100% {
            -webkit-transform: rotate(360deg);
        }
    }
    @keyframes spin {
        0% {
            transform: rotate(0deg);
        }
        100% {
            transform: rotate(360deg);
        }
    }
    .toggle-btn {
        float: left; margin-left: 20px;
        margin-bottom: 10px;
         cursor: pointer;
        padding: 8px;
        font-size: 20px;
        
    }
    .author {
        font-size: 16px;
        font-weight: 300;
        position: absolute;
        top: 50%;
        left: 15%;
    }
    .rotate {
        transform: translateX(-50%) translateY(-50%) rotate(90deg);
    }
    @media screen and (max-width: 800px) {
        .row {
            display: block;
        }
    }
</style>