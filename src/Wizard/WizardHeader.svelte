<script>
	export let currentStep = undefined // store
	export let wizard = undefined // store
</script>

<header class="bg-theme-primary p-4 rounded-t-md md:flex">
	{#each $wizard.steps as step, index (step.key)}
		<button type="button" on:click={() => $currentStep = $wizard.steps[index]}
			class="text-blue-200 mx-2 font-bold p-1 list-none md:flex-grow text-lg md:text-base {$currentStep.key === step.key ? '!text-white font-bold' : 'hidden md:block'}">
			{#if $wizard.mode === 'wizard'}
				Step {index + 1}
			{:else}
				{step.header}
			{/if}
		</button>
	{/each}
</header>