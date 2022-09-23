<script>
  function convertNameToLabel (name) {
    return (
      name.charAt(0).toUpperCase() +
      name
        .slice(1)
        .replace(/([A-Z]+)/g, ' $1')
        .replace(/([A-Z][a-z])/g, ' $1')
    )
  }

  import { createEventDispatcher } from 'svelte'
  import { onMount } from 'svelte'
  import Label from '../Forms/Label.svelte'

  export let id = ''
  export let name = ''
  export let label = convertNameToLabel(name)
  export let value = undefined
  export let help = undefined
  export let disabled = undefined
  export let autosize = false
  export let required = undefined
  export let error = undefined
  export let errors = undefined
  export let rows = undefined
  let input
  const dispatch = createEventDispatcher()
  export const focus = () => input.focus()
  export const select = () => input.select()
  $: props = {
    ...$$restProps,
    class: 'form-textarea'
  }
  onMount(() => {
    if (autosize) {
      fitTextarea.watch(input)
    }
  })

  function update (event) {
    value = event.target.value
    dispatch('input', event)
  }

  function blur (event) {
    value = event.target.value
    dispatch('blur', event)
  }
</script>

<div class={$$restProps.class}>
  <Label class="mb-1" {label} {id} {required} />

  <div class="input-wrapper">
    <textarea {...props} {name} {rows} bind:this={input} class:error class:disabled {id} {value} on:blur={blur} on:input={update} {required} />
  </div>

  {#if help }
    <p class="text-sm text-gray-600 italic">{help}</p>
  {/if}
  <div class="input-wrapper">
    {#if error}
      <span class=" text-sm text-theme-danger">{error}</span>
    {/if}
    {#if $errors && $errors[name]}
      <span class="text-sm text-theme-danger text-red-500">
        {$errors[name]}
      </span>
    {/if}
</div>
</div>