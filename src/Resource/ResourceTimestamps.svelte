<script>
  import { getContext } from 'svelte'
  import Timestamp from '@/components/Date/Timestamp.svelte'
  import UserLink from '@/pages/users/_users.UserLink.svelte'

  export let resource = Object.values($$restProps)[0] || getContext('resource') || {}
  const updatedBy = resource.meta?.updatedBy
</script>

{#if resource.createdAt}
  <span>
    Created: <Timestamp timestamp={resource.createdAt}/>
    {#if updatedBy && !resource.updatedAt}
      by <UserLink userId={updatedBy}/>
    {/if}
  </span>
{/if}
{#if resource.updatedAt}
  <span>
    Updated: <Timestamp timestamp={resource.updatedAt} format="timeToNow"/>
    {#if updatedBy}
      by
      <UserLink userId={updatedBy}/>
    {/if}
  </span>
{/if}
