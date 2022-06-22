<script>
  export let id = undefined
  export let label = undefined
  export let required = undefined
</script>

{#if label}
  <label for={id} class="block text-sm font-semibold text-gray-700 {$$restProps.class}">
    {label}
    {#if !required}
      <span class="text-xs italic text-gray-400">(Optional)</span>
    {/if}
  </label>
{/if}