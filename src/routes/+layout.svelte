<script>
	import './styles.css'
</script>

<div>
	<slot />
</div>

<style label='postcss'>
	:global(body) {
		background-color: #252525;
		margin: 0;
	}
</style>