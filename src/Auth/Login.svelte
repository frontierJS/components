<script context="module">
  const make = (spec) => ({
    email: '',
    password: '',
    ...spec
  })
</script>

<script>
  // import { login } from '@/core/auth'
  import { auth } from '@/core/frontier'
  import { title } from '@/core/app'
  import Input from '@/components/Forms/Input.svelte'
  import { useForm } from '@/components/Forms/Form.svelte'
  import Logo from '@/components/Logo.svelte'
  import { goto } from '@/core/router'

  $title = 'Login'

  const signIn = make()

  const { form, status, errors } = useForm({
    // debug: true,
    finishDelay: 100,
    submit: () => auth.login(signIn, () => $goto('/home')),
    // We manually set errors because we aren't getting errors.data from the actual form fields themselves
    afterErrors: (errs) => ($errors = errs),
    afterChange: () => ($errors = {}),
    afterFinish: () => ($status = 'One moment...')
  })
</script>

<div class="min-h-screen bg-gray-50 py-12 sm:px-6 lg:px-8">
  <div class="sm:mx-auto sm:max-w-md sm:w-full">
    <Logo class="mx-auto h-12 w-auto" />
    <h2 class="font-extrabold mt-6 text-center text-3xl text-gray-900">
      Sign in to your account
    </h2>
  </div>

  <div class="bg-white shadow mt-8 py-8 px-4 sm:rounded-lg sm:mx-auto sm:max-w-md sm:w-full sm:px-10">
      <form id="sign-in-form" use:form={signIn} class="space-y-4 form">
        <Input
          bind:value={signIn.email}
          name="email"
          type="email"
          required
          {errors}
        />
        <Input
          bind:value={signIn.password}
          name="password"
          type="password"
          required
          minlength="8"
          {errors}
        />
        <!-- <div class="flex items-center justify-between">
          <div class="flex items-center">
            <input id="remember_me" name="remember_me" type="checkbox" class="rounded border-gray-300 h-4 text-indigo-600 w-4 focus:ring-indigo-500">
            <label for="remember_me" class="text-sm ml-2 text-gray-900 block">
              Remember me
            </label>
          </div> -->

        <div>
          <button type="submit" >
            {$status || 'Sign In'}
          </button>
          {#if $errors?.message}
            <span class="text-theme-danger ml-4">{$errors.message}</span>
          {/if}
        </div>
      </form>
      <div class="mt-2 text-sm">
        <a href="/register" class="mr-2">
          Sign Up
        </a>
        <!-- <a href="/forgot">
          Forgot your password?
        </a> -->
      </div>
  </div>
</div>
