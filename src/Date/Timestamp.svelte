<script>
  export let timestamp
  export let format = ''

  const date = timestamp ? new Date(timestamp) : null
  const formatted = date ? date.format('DDD, MMM DD, YYYY h:mm aa') : ''
</script>

{#if format === 'timeToNow'}
  <span title={formatted} class="italic text-theme-light">{date?.relativeToNow()} {timestamp ? 'ago' : '' }</span>
{:else}
  <span title={formatted} class="italic text-theme-light">{formatted}</span>
{/if}