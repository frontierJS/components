<script>
	export let wizard = undefined // store
</script>

<footer class="p-4 flex">
	{#if !$wizard.onFirstStep}
		<button type="button" class="back btn secondary" on:click={() => $wizard.previous()}>{$wizard.backText}</button>
	{/if}
	<div class="ml-auto space-x-2">
		{#if $wizard.skippableStep}
			<button type="button" on:click={() => $wizard.next()} class="btn secondary">
				{$wizard.skipText}
			</button>
		{/if}
		<button type="submit" class="next primary">
			{$wizard.nextText}
		</button>
	</div>
</footer>