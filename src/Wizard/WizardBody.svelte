<script>
	export let currentStep = undefined // store
	export let wizard = undefined // store
	export let layout = undefined
</script>

<div class="body p-8">
	<h2 class="p-0">{$currentStep.title || $currentStep.header}</h2>
	{#if $currentStep.subtitle}
		<p class="pb-4">{$currentStep.subtitle}</p>
	{/if}
	<div class="content {$currentStep.key}">
		{#if layout}
			<svelte:component this={layout}>
				<svelte:component this={$wizard.currentComponent} />
			</svelte:component>
		{:else}
			<svelte:component this={$wizard.currentComponent} />
		{/if}
	</div>
</div>

<style>

</style>