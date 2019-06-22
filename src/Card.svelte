<script>
  export let data = [];
  let index = 0;
  function forward() {
    if (index < data.length - 1) {
      index++;
      speak(data[index].title);
    }
  }
  function backward() {
    if (index > 0) {
      index--;
      speak(data[index].title);
    }
  }
  function handleKey(event) {
    if (event.keyCode === 39) {
      forward();
    } else if (event.keyCode === 37) {
      backward();
    }
  }
  function speak(str) {
    let msg = new SpeechSynthesisUtterance(str);
    window.speechSynthesis.speak(msg);
  }
</script>

<style>
  a {
    display: block;
    width: 100%;
    padding: 30px 0;
    color: aliceblue;
    background-color: rgb(0, 0, 0, 0.5);
  }

  button {
    width: 60px;
    height: 60px;
    border-radius: 60px;
    background-color: #ff9900;
    color: white;
  }
  .card {
    display: flex;
    height: 100%;
    width: 500px;
    border-radius: 15px;
    align-items: center;
    position: relative;
    background-size: cover;
    background-repeat: no-repeat;
  }
  .backward-btn {
    position: absolute;
    left: 25%;
    top: 75%;
  }
  .forward-btn {
    position: absolute;
    right: 25%;
    top: 75%;
  }
</style>
<svelte:head>
<link href="https://fonts.googleapis.com/icon?family=Material+Icons"
      rel="stylesheet">
</svelte:head>
<svelte:window on:keydown={handleKey}/>

<div class="card" style="background-image: url({data[index].urlToImage})">
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
