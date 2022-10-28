<script>
	import { getContext } from 'svelte';
  import GridItem from '../components/GridItem.svelte';

  //Get the scroll instance
	const { getScroll } = getContext('locomotiveScroll');
	const scrollInstance = getScroll();

	//get data from parent component
	export let dogs;

	import { onMount } from 'svelte';

	//clamp the value to keep skewing smooth
	const clamp = (value, min, max) => (value <= min ? min : value >= max ? max : value);

	//Locomotive Scroll Refs
	let scroll = {
		cache: 0,
		current: 0
	};

	let reference;
	let leftColumnRef;
	let middleColumnRef;
	let rightColumnRef;
	//Split the data in 3
	const leftChunk = [...dogs].splice(0, 5);
	const middleChunk = [...dogs].splice(5, 5);
	const rightChunk = [...dogs].splice(10, 5);
	//Initialize scroll object on component mount
	onMount(() => {
		scrollInstance.on('scroll', (obj) => {
			if (leftColumnRef) {
				// Find distance between scroll updates
				scroll.current = obj.scroll.y;
				const distance = scroll.current - scroll.cache;
				scroll.cache = scroll.current;
				leftColumnRef.style.transform = `skewY(${clamp(distance, -10, 10)}deg)`;
				rightColumnRef.style.transform = `skewY(${clamp(distance, -10, 10)}deg)`;
				middleColumnRef.style.transform = `skewY(${clamp(-distance, -10, 10)}deg)`;
			}
		});
	});
</script>

<!-- returning property data into your component -->

<svelte:head>
	<title>Home</title>
</svelte:head>

	<div class="grid-wrap">
		<div class="left-column" bind:this={leftColumnRef}>
			{#each leftChunk as { url, description }, i}
				<GridItem {url} {description} index={i} />
			{/each}
		</div>
		<div class="middle-column" data-scroll data-scroll-speed="-20">
			<div bind:this={middleColumnRef}>
				{#each middleChunk as { url, description }, i}
					<GridItem {url} {description} index={i} />
				{/each}
			</div>
		</div>
		<div class="right-column" bind:this={rightColumnRef}>
			{#each rightChunk as { url, description }, i}
				<GridItem {url} {description} index={i} />
			{/each}
		</div>
	</div>

<style lang="scss">
	.grid-wrap {
		display: grid;
		grid-template-columns: repeat(3, 20%);
		gap: 5%;
		justify-content: center;
		margin: 0 auto;
	}
</style>
