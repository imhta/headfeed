<script>
	import {onMount} from 'svelte';

	let data = [];
	let index = 0;
	onMount(async function () {
		const res = await fetch('https://hacker-news.firebaseio.com/v0/topstories.json');
		const jsonIdData = await res.json();
		jsonIdData.slice(0, 19).forEach(async (id) => {
			const res = await fetch(`https://hacker-news.firebaseio.com/v0/item/${id}.json`)
			const jsonNews = await res.json();
			data = [...data, jsonNews]; 
		});
	});
	function forward() {
		if(index < data.length) {
			index++;
		}
	}
	function backward() {
		if(index > 0) {
			index--;
		}
	}
	function handleKey(event) {
		if(event.keyCode === 39) {
			if(index < data.length) {
				index++;
			}
		}else if (event.keyCode === 37) {
			if(index > 0) {
				index--;
			}
		}
	}
</script>

<style>
	h1, h2 {
		text-align: center;
	}
	a {
		display: block;
		margin: 10px;
	}
	.btn-wrapper {
		display: flex;
		justify-content: center;
	}

</style>
<nav><h1>headfeed</h1></nav>
<main>
{#if data.length}
<h2><a href="{data[index].url}" >{index + 1}. &nbsp;{data[index].title}</a></h2>
<div class="btn-wrapper">
<button on:click={backward}>Backward</button><button on:click={forward}>Forward</button>
</div>
{:else} 
<h2>loading..</h2>
{/if}

</main>