<script>
  import * as animateScroll from "svelte-scrollto";

</script>
<nav class="hidden text-gray-500 uppercase text-bold sm:block" style="color: #f3f3f3; background-color: #222222">
    <p on:click={() => animateScroll.scrollTo({element: '#article-section', duration: 2000, offset: -600})}
       class="hover:text-gray-700 hover:no-underline" style="padding: 10px"></p>
      <p on:click={() => animateScroll.scrollTo({element: '#article-section', duration: 2000, offset: -600})}
         class="hover:text-gray-700 hover:no-underline" style="padding: 10px"></p>
  </nav>

  <style>
    p {
      cursor: pointer;
    }
  </style>