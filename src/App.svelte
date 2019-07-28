<script>
  import { onMount } from "svelte";
  import Card from "./Card.svelte";
  import config from "../config.js";
  let data = [];

  onMount(async function() {
    getTopFromNewsAPI();
  });
  async function getTopFromNewsAPI() {
    const api = config.MY_API_KEY;
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
  :global(h2) {
    text-align: center;
  }
  nav h2 {
    font-size: 30px;
  }
  h2 span {
    color: #ff9900;
  }
  
  .btn-wrapper {
    display: flex;
    width: 100%;
    justify-content: center;
  }
  nav button{
    padding: 5px 10px;
    outline: none;
    border: none;
  }
  button.selected{
    background-color: #ff9900;
    color: black;
  }
  main {
    display: flex;
    height: 75%;
    justify-content: center;
  }
</style>

<nav>
  <h2>head<span>feed</span></h2>
  <div class="btn-wrapper">
  <button class="selected">India</button>
  <button>US</button>
  <button>Business</button>
  <button>Technology</button>
  </div>

</nav>

<main>

  {#if data.length}
    <Card {data} />
  {:else}
    <h2>loading..</h2>
  {/if}

</main>
