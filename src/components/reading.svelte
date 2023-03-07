<script>
    import { fade } from "svelte/transition";
    import { onMount } from "svelte";

    import Accordion from './accordion.svelte'
    
    // import d3 library
    import * as d3 from "d3";

    // store URLs as variables for future reference
    const pastReadFP =
        "https://docs.google.com/spreadsheets/d/e/2PACX-1vSmCQpEj8lo3AVss7eQdxDx-MW4d8uQO6cP39V6uGLUfjhDLDqGPJQHsgMLZjhopgxuVqWBfrLmkoaA/pub?output=csv";

    const highlightsFP = "https://docs.google.com/spreadsheets/d/e/2PACX-1vRFBL3j0ypihBCybuY2kMLddwyD34rKMJCMfcHQ0ydrJtroxXxATMdEkO_pJrm8Nh1KfmV8hZaYbhYd/pub?output=csv"

    let dataLoaded = false;

    const articles = [];
    const highlights = [];
    onMount(async () => {
        console.log()
        // use d3 csv to read fp variable
        Promise.all([
        d3.csv(pastReadFP),
        d3.csv(highlightsFP)
      ]).then((datasets) => {
        console.log(datasets)

        // first store the highlights in an arr of objects
        datasets[1].forEach(d => {
            let obj = {};
            
            obj.text = Object.values(d)[2];
            obj.title = Object.values(d)[1];

            highlights.push(obj)
        })
        console.log(highlights)

        datasets[0].forEach(d => {
            let obj = {}
            // get first item in object d
            obj.date = Object.values(d)[0];

            if (Object.values(d)[1].length > 90) {
                obj.title = Object.values(d)[1].substring(0, 89) + "...";
            } else {
                obj.title = Object.values(d)[1];
            }
            
            obj.preview = Object.values(d)[2];
            obj.link = Object.values(d)[3];
            // console.log(highlights.find(h => h.title == Object.values(d)[1]))
            obj.highlights = highlights.filter(h => h.title == Object.values(d)[1])

            articles.push(obj)
        })
        
        console.log(articles)
        // reverse order of articles
        articles.reverse()

        dataLoaded = true;
      })

      
    });
   
    
</script>

<svelte:head>
    <title>Reading</title>
</svelte:head>

<div>   
    <p>Pieces I have gathered online and read on Instapaper. Click EXPAND to see my highlights.</p>
    <br>
    <br>
    <div id="article-section" in:fade class="row">
        {#if dataLoaded != false}
            {#each articles as article}
                <div class="article-sum">
                    <a href="{article.link}">
                        <h3 style="text-decoration: underline;">{article.title}</h3>
                    </a>
                    <span class="date-tag">{article.date}</span>
                </div>
                {#if article.highlights.length > 0}
                <div>Highlights</div>
                <div>
                    {#each article.highlights as highlight}
                        <div class="highlight-marker">

                        </div>
                    {/each}
                    
                    <Accordion entry={article.highlights} title="expand" />
                </div>
                {/if}
                <hr />
            {/each}
        {/if}
    </div>
</div>

<style>
    hr {
        margin-top: 10px;
        border: 1px solid  #222222;
        height: 1px
    }
    a { color: inherit; } 

    h2 {
        font-size: 2rem;
    }

    h3 {
        font-size: 1.5rem;
        font-weight: 300
    }

    .article-sum {
        margin-top: 20px;
        margin-bottom: 10px;
    }

    .highlight-marker {
        display: inline-block;
        background-color: #f3f3f3; 
        width: 10px;
        height: 10px;
        border-radius: 10px;
        margin: 3px;
    }

    .date-tag {
        font-size: 12px;
        font-style: italic;
    }
</style>