<script>
  import Field from '@/components/Forms/Field.svelte'

  export let id = ''
  export let value = ''
  export let name = ''
  export let label = undefined
  export let pattern = undefined
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

<Field {name} {label} {error} {errors} {help} {copyFeature}>
  <input {name} {...props} bind:this={input} class:error {id} {type} {value} {disabled} {pattern} class:disabled on:input={update} on:blur on:keypress class="" {required} />
</Field>
