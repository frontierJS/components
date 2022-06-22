<script>
  export let text = undefined
  export let title = undefined
</script>


<sup {title} class="bg-white rounded-md text-xs py-1 px-2">
  <slot>
    {text}
  </slot>
</sup>