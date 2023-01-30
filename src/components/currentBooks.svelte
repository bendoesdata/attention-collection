<script>
    import { fade } from "svelte/transition";
    import { onMount } from "svelte";
    // store URLs as variables for future reference
    const currentlyReadingUrl =
        "https://openlibrary.org/people/bendextercooley/books/currently-reading.json";
    const coverUrl = "http://covers.openlibrary.org/b/olid/";
    const olBookUrl = "https://openlibrary.org/books/";
    // create some empty variables for storing data later
    // these need to be before onMount so they are global, and can be used in the template
    let formattedReading = [];
    let currentList = [];
    let finalBooks, nowReading;
    onMount(async () => {
        currentList = await fetch(currentlyReadingUrl).then((x) => x.json());
        nowReading = formatData(currentList, formattedReading);
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
        // pull out relevant data
        dataObject.reading_log_entries.forEach((d) => {
            let obj = {};
            if (d.logged_edition == null) {
                obj.ol_id = d.work.key.split("/")[2];
            } else {
                obj.ol_id = d.logged_edition.split("/")[2];
            }
            (obj.title = d.work.title),
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
</script>

<svelte:head>
    <title>Currently reading</title>
</svelte:head>

<div>
    <div>
        <div>
            <h3 style="padding: 20px; font-weight: 600">Books</h3>
            <div style="min-height: 200px; margin-top: 20px">
                {#if nowReading != null}
                    {#each nowReading as book}
                        <div in:fade
                            style="display: inline-block; margin-left: 20px; margin-right: 20px"
                        >
                            <a
                            href="{olBookUrl}{book.ol_id}"
                            target="_blank" rel="noreferrer">
                                <img 
                                    style="margin-bottom: 10px"
                                    width="120"
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

    
</div>

<style>    
    a { color: inherit; } 
    
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