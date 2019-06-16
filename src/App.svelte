<script>
  import { onMount } from "svelte";
  import Card from "./Card.svelte";

  let data = [];

  onMount(async function() {
    getTopFromNewsAPI();
  });
  async function getTopFromNewsAPI() {
    const api = "f4e25419df6f4a1aac2faaaf3ae149ff";
    const res = await fetch(
      `https://newsapi.org/v2/top-headlines?country=IN&category=technology&apiKey=${api}`
    );
    const jsonData = await res.json();
    data = jsonData.articles;
  }
  async function getHackerNews() {
    const res = await fetch("https://node-hnapi.herokuapp.com/news?page=0");
    const jsonNews = await res.json();
    data = jsonNews;
  }
</script>

<style>
  :global(h1) {
    text-align: center;
  }
  nav h1 {
    font-size: 40px;
  }
  h1 span {
    color: #ff9900;
  }

  main {
    display: flex;
    height: 75%;
    justify-content: center;
  }
</style>

<nav>
  <h1>head<span>feed</span></h1>
</nav>

<main>

  {#if data.length}
    <Card {data} />
  {:else}
    <h2>loading..</h2>
  {/if}

</main>
