<script context="module">
	import { writable, derived } from 'svelte/store'
	export { make }

	function make (steps, options = {}) {
	  const { components, initialStepKey, defaultSkippable = false, mode = 'wizard' } = options

	  let index = steps.findIndex((step) => step.key === initialStepKey)
	  index = index < 0 ? 0 : index
	  const currentStep = writable(steps[index])

	  const wizard = derived(currentStep, ($currentStep) => {
	    const currentStepIndex = steps.indexOf($currentStep)

	    return {
	      steps,
	      mode,
	      onFirstStep: currentStepIndex === 0,
	      onLastStep: currentStepIndex === steps.length - 1,
	      notOnFirstOrLastStep: currentStepIndex !== 0 && currentStepIndex !== steps.length - 1,
	      skippableStep: $currentStep.skippable || defaultSkippable,
	      nextText: $currentStep.nextText || (mode === 'wizard' ? 'Next' : 'Continue'),
	      backText: $currentStep.backText || 'Back',
	      skipText: $currentStep.skipText || 'Skip',
	      index: currentStepIndex,
	      currentStep: $currentStep,
	      currentComponent: components[$currentStep.component],
	      next: (count = 1) => currentStep.set(steps[currentStepIndex + count]),
	      previous: (count = 1) => currentStep.set(steps[currentStepIndex - count]),
	      goToStep: (key) => currentStep.set(steps.find((step) => step.key === key))
	    }
	  })

	  return {
	    wizard,
	    currentStep
	  }
	}
</script>

<script>
  import WizardHeader from './WizardHeader.svelte'
  import WizardBody from './WizardBody.svelte'
  import WizardFooter from './WizardFooter.svelte'

	export let currentStep = undefined // store
	export let wizard = undefined // store
	export let layout = undefined
</script>

<WizardHeader {currentStep} {wizard} />
<WizardBody {currentStep} {wizard} {layout} />
<WizardFooter {wizard} />