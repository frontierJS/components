<script>
  import Navigation from './Navigation.svelte'
  import NavFooter from './NavFooter.svelte'
  import Logo from '../Logo.svelte'
  import Icon from '../Icon.svelte'
  import { sidebarState } from '@/core/app'
  import { route } from '@/core/router'
</script>

<!-- Off-canvas menu for mobile, show/hide based on off-canvas menu state. -->
<div class:hidden={$sidebarState === 'hidden' || !$sidebarState} class="fixed inset-0 flex z-40 md:hidden" role="dialog" aria-modal="true">
  <!--
      Off-canvas menu overlay, show/hide based on off-canvas menu state.

      Entering: "transition-opacity ease-linear duration-300"
        From: "opacity-0"
        To: "opacity-100"
      Leaving: "transition-opacity ease-linear duration-300"
        From: "opacity-100"
        To: "opacity-0"
    -->
  <div class="fixed inset-0 bg-gray-600 bg-opacity-75" aria-hidden="true" />

  <!--
      Off-canvas menu, show/hide based on off-canvas menu state.

      Entering: "transition ease-in-out duration-300 transform"
        From: "-translate-x-full"
        To: "translate-x-0"
      Leaving: "transition ease-in-out duration-300 transform"
        From: "translate-x-0"
        To: "-translate-x-full"
    -->
  <div class="relative flex-1 flex flex-col max-w-xs w-full bg-theme-nav">
    <!--
        Close button, show/hide based on off-canvas menu state.

        Entering: "ease-in-out duration-300"
          From: "opacity-0"
          To: "opacity-100"
        Leaving: "ease-in-out duration-300"
          From: "opacity-100"
          To: "opacity-0"
      -->
    <div class="absolute top-0 right-0 -mr-12 pt-2">
      <button on:click={ () => $sidebarState = 'hidden'} class="ml-2 flex items-center justify-center h-12 w-12 rounded-full focus:outline-none focus:ring-2 focus:ring-inset focus:ring-white">
        <span class="sr-only">Close sidebar</span>
        <Icon name="x" class="" />
      </button>
    </div>

    <div class="flex-1 h-0 pt-5 pb-4 overflow-y-auto">
      <div class="flex-shrink-0 flex items-center px-4">
        <Logo type="blue-on-white" />
      </div>
      {#if $route.shortPath !== '/setup'}
        <Navigation />
      {/if}
    </div>
    <NavFooter />
  </div>

  <div class="flex-shrink-0 w-14" aria-hidden="true">
    <!-- Force sidebar to shrink to fit close icon -->
  </div>
</div>
