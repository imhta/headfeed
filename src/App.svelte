<script>
  import { onMount } from "svelte";

  let data = [];
  let index = 0;
 
  onMount(async function() {
    getTopFromNewsAPI();
  });
async function getTopFromNewsAPI() {
    const api = 'f4e25419df6f4a1aac2faaaf3ae149ff';
    const res = await fetch(`https://newsapi.org/v2/top-headlines?country=us&category=business&apiKey=${api}`)
    const jsonData = await res.json();
    data = jsonData.articles;
}
async function getHackerNews() {
    const res = await fetch(
      "https://hacker-news.firebaseio.com/v0/topstories.json"
    );
    const jsonIdData = await res.json();
    jsonIdData.slice(0, 21).forEach(async id => {
      const res = await fetch(
        `https://hacker-news.firebaseio.com/v0/item/${id}.json`
      );
      const jsonNews = await res.json();
      data = [...data, jsonNews];
    });
  }
  function forward() {
    if (index < data.length-1) {
      index++;
      speak(data[index].title)
    }
  }
  function backward() {
    if (index > 0) {
      index--;
    speak(data[index].title)
    }
  }
  function handleKey(event) {
    if (event.keyCode === 39) {
      if (index < data.length) {
        index++;
      }
    } else if (event.keyCode === 37) {
      if (index > 0) {
        index--;
      }
    }
  }
  function speak(str) {
      let msg = new SpeechSynthesisUtterance(str);
      window.speechSynthesis.speak(msg);
  }
</script>

<style>
   .material-icons{font-size: 40px;}
  h1{
    text-align: center;
  }
  nav h1{
    font-size: 40px;
  }
  h1 span{
    color: #FF9900;
  }
  a {
    display: block;
    margin: 10px;
  }

  button{
    width: 60px;
    height: 60px;
    border-radius: 60px;
    background-color: #FF9900;
    color: white;
   }
   main{
     display: flex;
     height: 75%;
     justify-content: center;
     }
   .card{
     display: flex;
     height: 100%;
     width: 500px; 
     /* border-radius: 15px; */
     align-items: center;
     background-color: bisque;
     position: relative;
   }
  .backward-btn{
    position: absolute;
    left: 25%;
    top: 75%;
  }
  .forward-btn{
    position: absolute;
    right: 25%;
    top: 75%;
  }
</style>
<link href="https://fonts.googleapis.com/icon?family=Material+Icons"
      rel="stylesheet">

<nav>
  <h1>head<span>feed</span></h1>

</nav>

<main>
  {#if data.length}
   <img src="{data[index].urlToImage}" alt="{data[index].title}">
  <div class="card">

    <h1>
      <a href={data[index].url}>{data[index].title}</a>
    </h1>
      <button class="backward-btn" on:click={backward}>
            <i class="material-icons">keyboard_arrow_left</i>
      </button>
      <button class="forward-btn" on:click={forward}>
            <i class="material-icons">keyboard_arrow_right</i>
      </button>
    </div>



  {:else}
    <h2>loading..</h2>
  {/if}

</main>
