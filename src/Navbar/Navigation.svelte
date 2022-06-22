<script>
  import { beforeUrlChange } from '@/core/router'
  import Icon from '@/components/Icon.svelte'
  import { sidebarState, currentUser } from '@/core/app'
  import NavLink from './NavLink.svelte'
  import { store as stats } from '@/resources/Stat.svelte'

  $beforeUrlChange(() => {
    $sidebarState = 'hidden'
    return true
  })
</script>

<nav class="mt-4 px-2 space-y-1">
  <a id="leads-create" href="/leads/create"
      class="bg-white text-theme-primary group flex items-center px-2 py-2 text-base font-medium rounded-md">
    <Icon name="user-add" class="mr-4 h-6 w-6"/>
    New Lead
  </a>
  <NavLink id="home" route="/home" text="Dashboard" icon="presentation"/>
  <NavLink id="leads-index" route="/leads" text="Leads" icon="users" count={$stats.counts?.leads}/>
  <NavLink id="clients-index" route="/clients" text="Clients" icon="users" count={$stats.counts?.clients} />
  <!--  <NavLink id="properties-index" route="/properties" text="Properties" icon="home" count={$stats.counts?.properties} />-->
  <!-- <NavLink id="notes-index" route="/notes" text="Notes" icon="annotation" /> -->
  <!-- <NavLink id="estimates-index" route="/estimates" text="Estimates" icon="newspaper" /> -->
  <NavLink id="boards-index" route="/boards" text="Boards" icon="board"/>
  <!-- <NavLink id="tasks-index" route="/tasks" text="Tasks" icon="clipboard-check" /> -->
  <NavLink id="forms-index" route="/forms" text="Forms" icon="clipboard-list"/>
  <NavLink id="templates-index" route="/templates" text="Templates" icon="mail"/>
  <NavLink id="users-index" route="/users" text="Users" icon="user-circle" count={$stats.counts?.users}/>
  <NavLink id="sites-index" route="/sites" text="Sites" icon="template"/>
  {#if $currentUser.isAdmin}
    <NavLink id="account-index" route="/account" text="Settings" icon="cog"/>
  {/if}
</nav>
