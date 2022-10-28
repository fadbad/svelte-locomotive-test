<script>
	export let description, url;
</script>

<div class="grid-item">
	<img class="grid-item-media" src={url} alt={url} />
	<p>{description}</p>
</div>

<style lang="scss">
  .grid-item {
    margin-bottom: 20px;
  }
	.grid-item img {
			width: 100%;
			object-fit: cover;
			aspect-ratio: 1;
		}

		p {
			margin-top: 15px;
			font-size: 24px;
			text-transform: uppercase;
			text-align: right;
		}
</style>
