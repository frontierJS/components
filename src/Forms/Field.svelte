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
  export let name = ''
  export let label = convertNameToLabel(name)
  export let help = ''
  export let required = undefined
  export let error = ''
  export let errors = undefined
  export let copyFeature = false
</script>

<div class={'field ' + $$restProps.class}>
  <Label class="mb-1" {label} {id} {required} />

  <div class="field-wrapper relative">
    <slot/>
    {#if copyFeature}
      {@html window.copyByIdLink(id)}
    {/if}
  </div>

  <slot name="help">
    {#if help}
      <p class="text-sm pt-2 italic text-gray-600">{help}</p>
    {/if}
  </slot>

  <div class="field-wrapper">
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
