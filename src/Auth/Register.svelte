<script context="module">
  const make = (spec) => ({
    companyName: '',
    firstName: '',
    lastName: '',
    email: '',
    password: '',
    ...spec
  })
</script>

<script>
  import { onMount } from 'svelte'
  import { currentUser } from '@/core/app'
  // import { register } from '@/core/auth'
  import { auth } from '@/core/frontier'
  import { goto } from '@/core/router'
  import Input from '../Forms/Input.svelte'
  import { useForm } from '../Forms/Form.svelte'
  import Logo from '@/components/Logo.svelte'

  // if currentUser is already logged in, we don't want them on the register page
  $: if ($currentUser) $goto('/home')

  onMount(() => {
    if ($currentUser) $goto('/home')
  })
  const registrant = make()

  const { form, errors, status } = useForm({
    // debug: true,
    submit: () => auth.register(registrant, () => $goto('/setup'))
  })
</script>

<!-- routify:options index=false -->
<div class="min-h-screen bg-gray-50 py-12 sm:px-6 lg:px-8">
  <div class="sm:mx-auto sm:w-full sm:max-w-md">
    <Logo class="mx-auto h-12 w-auto" />
    <h2 class="mt-6 text-center text-3xl font-extrabold text-gray-900">
      Create your account
    </h2>
  </div>

  <div
    class="w-full mt-8 bg-white py-8 px-4 shadow sm:mx-auto sm:max-w-md sm:rounded-lg sm:px-10"
  >
    <form id="registrant-form" use:form={registrant} class="form space-y-4">
      <Input
        name="companyName"
        required
        minlength="1"
        {errors}
        bind:value={registrant.companyName}
      />
      <Input
        name="firstName"
        required
        minlength="1"
        {errors}
        bind:value={registrant.firstName}
      />
      <Input
        name="lastName"
        required
        minlength="1"
        {errors}
        bind:value={registrant.lastName}
      />
      <Input
        name="email"
        label="Email"
        type="email"
        required
        {errors}
        bind:value={registrant.email}
      />
      <Input
        name="password"
        label="Password"
        type="password"
        required
        minlength="8"
        {errors}
        bind:value={registrant.password}
      />
      <button type="submit" >
      {#if $status === 'submitting'}
        Submitting
      {:else}
        Sign Up
      {/if}
      </button>
    </form>

    <a href="/login" class="block text-sm mt-2"> Sign In </a>
  </div>
</div>

<style>
</style>
