<script>
  import { feature } from '@/core/actions'
  import Navigation from './Navigation.svelte'
  import NavFooter from './NavFooter.svelte'
  import Logo from '../Logo.svelte'
  import { route } from '@/core/router'

  // This hide nav feature is in beta
  let navEl
  let navHidden = false
  function hideNav() {
    navEl.style['margin-left'] = '-180px'
    navHidden = true
  }
  function showNav() {
    if (navHidden === true) {
      navEl.style['margin-left'] = 'unset'
      navHidden = false
    }
  }
</script>

<div bind:this={navEl} on:click={showNav} class="hidden flex flex-col md:w-50 bg-theme-nav md:flex md:flex-shrink-0">
  <div class="flex flex-col h-0 flex-1">
    <div class="flex-1 flex flex-col pt-5 pb-4 overflow-y-auto">
      <div class="flex items-center flex-shrink-0 px-4">
        <Logo type="blue-on-white" />
      </div>
      {#if $route.shortPath !== '/setup'}
        <Navigation />
        <button use:feature class="link text-theme-primary hover:text-white" on:click|stopPropagation={hideNav}>Hide nav</button>
      {/if}
    </div>
    <NavFooter />
  </div>
</div>