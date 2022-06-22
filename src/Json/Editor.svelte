<script>
  import { createEventDispatcher } from 'svelte'

  export let validJson = true
  export let json = {}
  export let help = ''
  const dispatch = createEventDispatcher()
  $: data = JSON.stringify(json, undefined, 4)

  function handleChange (e) {
    const result = JSON.try(e.target.value)
    if (result) {
      validJson = true
      json = result
      return dispatch('change', { json })
    }

    validJson = false
  }
</script>

<textarea class="border-4 {validJson ? '!border-green-400' : '!border-red-400' }" name="" id="" cols="30" rows="10" on:input={handleChange} value={data}></textarea>
{#if help}
  <p class="text-sm text-gray-600 italic">{help}</p>
{/if}