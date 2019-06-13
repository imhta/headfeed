<script>
	import {onMount} from 'svelte';

	export let name = "Hello world";
	let data = [];
	onMount(async function () {
		const res = await fetch('https://hacker-news.firebaseio.com/v0/topstories.json');
		const jsonIdData = await res.json();
		jsonIdData.slice(0, 20).forEach(async (id) => {
			const res = await fetch(`https://hacker-news.firebaseio.com/v0/item/${id}.json`)
			const jsonNews = await res.json();
			data = [...data, jsonNews]; 
		});
	});

</script>

<style>
	a {
		display: block;
		margin: 10px;
	}
</style>
<h1>headfeed</h1>
{#each data as news, i}
<a href="{news.url}">{i + 1}. &nbsp;{news.title}</a>
{/each}

