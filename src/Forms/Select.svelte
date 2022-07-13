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
  import Label from './Label.svelte'

  export let id = ''
  export let name = undefined
  export let label = convertNameToLabel(name)
  export let value = undefined
  export let help = undefined
  export let required = undefined
  export let error = undefined
  export let errors = undefined
  export const focus = () => input.focus()

  let input

  $: props = {
    ...$$restProps,
    class: 'form-select'
  }

  const dispatch = createEventDispatcher()

  function update (event) {
    value = event.target.value
    dispatch('change', event)
  }
</script>

<div class={$$restProps.class}>
  <Label class="mb-1" {label} {id} {required} />

  <select
    {...props}
    {name}
    bind:this={input}
    class:error
    {id}
    {value}
    {required}
    on:blur|preventDefault={update}
    on:change|preventDefault={update}
    class="select"
  >
    <slot selected={value} />
  </select>

  {#if help}
    <p class="text-sm pt-2 italic text-gray-600">{help}</p>
  {/if}

  <div>
    {#if error}
      {error}
      <span class=" text-sm text-theme-danger">{error}</span>
    {/if}
    {#if $errors && $errors[name]}
      <span class="text-sm text-theme-danger text-red-500">
        {$errors[name]}
      </span>
    {/if}
  </div>
</div>

<!-- Example

<Select name="template" label="Template" bind:value={email.templateId} on:change={changeTemplate}>
  <option value="">Choose a template...</option>
  {#each $templates as { id, name }}
    <option value={String(id)}>
      {name}
    </option>
  {/each}
</Select>

-->