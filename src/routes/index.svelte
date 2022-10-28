<script context="module">
	export async function load() {
		const url = `https://dog.ceo/api/breeds/image/random/15`;
		const res = await fetch(url);
		const data = await res.json();

		const loadedDogs = data.message.map((data, idx) => {
			return {
				url: data,
				id: idx + 1,
				description: `Block ${idx + 1}`
			};
		});

		return {
			props: { dogs: loadedDogs }
		};
	}
</script>

<script>
	export let dogs;
	import Home from '$lib/containers/Home.svelte';
</script>

<Home {dogs} />
