<script>
	export let view = 'default'
	export let blockTitle = ''

	const classes = (view) => {
	  const list = {
	    default: 'my-4 lg:flex lg:justify-between lg:items-center',
	    inline: 'flex items-center mb-2'
	  }

	  return list[view] + ' ' + $$restProps.class
	}
</script>

{#if view === 'default'}
	<header class={classes(view)}>
		<slot />
	</header>
{:else if view === 'inline'}
	<header class={classes(view)}>
		{#if blockTitle}
			<h2 id={blockTitle.toLowerCase()} class="mb-0 mr-2">{blockTitle}</h2>
		{/if}
		<slot />
  </header>
{:else}
	<header class={$$restProps.class}>
		<slot/>
	</header>
{/if}
