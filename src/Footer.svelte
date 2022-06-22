<script>
  import { getContext } from 'svelte'

export let show = getContext('view') !== 'wizard'

</script>

{#if show}
	<footer class={$$restProps.class}>
		<slot/>
	</footer>
{/if}
