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

  import Label from './Label.svelte'

  export let id = ''
  export let value = ''
  export let name = ''
  export let pattern = undefined
  export let label = convertNameToLabel(name)
  export let type = 'text'
  export let disabled = undefined
  export let help = ''
  export let required = undefined
  export let error = ''
  export let errors = undefined
  export let copyFeature = false
  export const focus = () => input.focus()
  export const select = () => input.select()

  let input

  $: props = {
    ...$$restProps,
    class: 'form-input'
  }

  function update (event) {
    if (type === 'number') {
      value = Number(event.target.value)
      return
    }
    value = event.target.value
  }
</script>

<div class={'input-wrapper ' + $$restProps.class}>
  <Label class="mb-1" {label} {id} {required} />

  <div class="input-wrapper relative">
    <slot>
      <input
        {name}
        {...props}
        bind:this={input}
        class:error
        {id}
        {type}
        {value}
        {disabled}
        {pattern}
        class:disabled
        on:input={update}
        on:blur
        on:keypress
        class=""
        {required}
      />
    </slot>
    {#if copyFeature}
      {@html window.copyByIdLink(id)}
    {/if}
  </div>

  <slot name="help">
    {#if help}
      <p class="text-sm pt-2 italic text-gray-600">{help}</p>
    {/if}
  </slot>

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
