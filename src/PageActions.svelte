<script>
  $: props = {
    class: 'fixed top-2 right-1rem md:right-2rem z-1000',
    ...$$restProps
  }
</script>

<div class={props.class}>
  <div class="float-right" style="display: flex; align-items: center;">
		<slot/>
  </div>
</div>