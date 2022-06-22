<script>
  import { goto } from '@/core/router'
  import { currentUser, currentAccount } from '@/core/app'
  import { auth } from '@/core/frontier'
  import Icon from '@/components/Icon.svelte'

  function handleLogout () {
    auth.logout(() => $goto('/index'))
  }
</script>

<div class="border-t border-blue-500 grid p-4">
  <div class="flex items-center">
    <Icon name="user" class="h-8 text-white w-8" />
    <div class="ml-3">
      <a href="/users/current" class="flex-shrink-0 w-full block">
        <p class="font-medium text-sm text-white">
          {$currentUser.fullName}
          <br>
          {$currentAccount.name || ''}
        </p>
        <p class="font-medium text-xs text-indigo-200">
          View profile
        </p>
      </a>
      <button type="button" class="font-medium text-xs p-0 link !text-indigo-200" on:click={handleLogout}>
        <!-- <Icon name="logout" class="h-6 stroke-white mr-4 w-6" /> -->
        Logout
      </button>
    </div>
  </div>
</div>