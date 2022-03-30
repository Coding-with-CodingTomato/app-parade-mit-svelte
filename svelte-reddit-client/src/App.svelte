<script>
let subReddit = "programmerhumor";
let posts = [];

const getPosts = async () => {
	const response = await fetch(`https://reddit.com/r/${subReddit}.json?limit=100`);
	const json = await response.json();

	posts = json.data.children;
};
</script>

<header class="titleBar">
	<h1>Svelte Reddit Client</h1>
	<div class="subredditWrapper">
		<p>reddit.com/r/</p>
		<input type="text" bind:value={subReddit} placeholder="programmerhumor" />
		<input type="button" on:click={getPosts} value="Get Posts" />
	</div>
</header>
<main>
	{#each posts as { data } (data.id) }
		<article>
			{#if data.post_hint === 'image'}
				<header class="imageWrapper">
					<img class="postImage" loading="lazy" src={data.url} alt={data.title} />
				</header>
			{/if}
			{data.title}
			<br /><br />
			<small>Von {data.author}</small>
			<br />
			{#if data.post_hint !== 'image'}
			<small><a target="_blank" href={data.url}>View post on Reddit</a></small>
			{/if}
		</article>
	{/each}
</main>
<footer>Made with ❤️</footer>

<style>

.subredditWrapper {
	display: flex;
	flex-direction: row;
	gap: 1rem;
	align-items: center;
}

.titleBar {
	margin: 0;
	padding: 0;
}

main {
	margin: 0;
	padding: 0;
	display: flex;
	flex-direction: column;
	gap: 2rem;
}

article {
	margin: 0;
	color: white;
}

.imageWrapper {
	display: flex;
	justify-content: center;
	align-items: center;
}
</style>