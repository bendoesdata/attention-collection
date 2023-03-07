<script>
    import { fade } from "svelte/transition";
    import { onMount } from "svelte";
    // store URLs as variables for future reference
    const pastReadUrl =
        "https://openlibrary.org/people/bendextercooley/books/already-read.json";
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
        bookList = await fetch(pastReadUrl).then((x) => x.json());
        finalBooks = formatData(bookList, formattedBooks);
        booksLength = finalBooks.length;
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

            if (obj.title != null) {
                arr.push(obj);
            }

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

    function imageFallback(val) {
        console.log('error', val)

        // img.addEventListener("error", function(event) {
        //     event.target.src = "https://default-image-link-goes-here"
        //     event.onerror = null
        // })
        
        console.log(val)
    }
</script>

<svelte:head>
    <title>Bookshelf</title>
</svelte:head>

<div id="book-page">
    <div in:fade class="row">
        <div>
            {#if finalBooks != null}
                <!-- {#if shelfView}
                    <button class="toggle-btn" on:click={toggleView}>Show as 📋 list</button>
                {:else}
                    <button class="toggle-btn" on:click={toggleView}>Show as 📚 bookshelf</button>
                {/if} -->
                <h3>Past read</h3>

                <span style="float: right; font-size: 20px">Total books: <strong>{booksLength}</strong></span>
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
                                        <a
                            href="{'https://bookshop.org/search?keywords='+book.title+' '+book.author}"
                            target="_blank">
                            <span class="fallback-title">{book.title}</span>
                                            <img 
                                        style="margin-bottom: 10px; width: 100px"
                                        src={book.cover_url}
                                        alt={book.title}
                                    />
                                    </a>
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
    #book-page {
        margin-top: 50px
    }
    
    /* a { color: inherit; }  */
    .book-row {
        /* border-bottom: 1px solid gray; */
        margin-bottom: 10px
    }

    .fallback-title {
        font-size: 14px;
        word-break: break-all;
        display: block;
        width: 100px;
        position: absolute;
        text-align: center;
        z-index: -1;

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
        margin: 0px;
        min-height: 500px;
        color: #f3f3f3
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