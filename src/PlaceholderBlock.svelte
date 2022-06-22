<script>
  export let title = ''
</script>

<div class="{$$restProps.class || ''}">
  <h2>{title}</h2>
  <p class="text-lg text-theme-info">
    <slot></slot>
  </p>
</div>

<style>
  div {
      @apply p-8 bg-gray-50 border-4 border-gray-200 border-dashed rounded-lg lg:flex-grow;
  }
</style>