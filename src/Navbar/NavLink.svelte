<script>
  import { isActive } from '@/core/router'
  import Icon from '../Icon.svelte'

  export let icon = undefined
  export let text = undefined
  export let id = undefined
  export let count = undefined
  export let route = undefined
  export let type = 'link'

  const classes = type === 'action' ? 'bg-white text-theme-primary' : 'text-white'
</script>

<a
  id={id}
  href={route}
  class:bg-theme-active={$isActive(route)}
  class="{classes} group flex items-center px-2 py-2 text-base font-medium rounded-md"
>
  {#if icon}
    <Icon name={icon} class="mr-4 h-6 w-6" />
  {/if}
  {text}
  {#if count}
    <span class="ml-2 px-2 rounded bg-theme-accent text-white">{count}</span>
  {/if}
</a>
