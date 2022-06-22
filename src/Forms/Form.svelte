<script context="module">
  import { createEventDispatcher } from 'svelte'
  import { writable } from 'svelte/store'


  export { useForm }

  function validateForm (form) {
    const elements = ['input', 'textarea', 'select'].flatMap((tag) => Array.from(form.getElementsByTagName(tag)))
    for (const el of elements) {
      if (el.willValidate && !el.checkValidity()) {
        el.classList.add('invalid')
      }
    }

    return form.reportValidity()
  }
  function getDebug (form) {
    if (form.data instanceof Promise || form.data.then) {
    return `
      <h3>Form Data is currently resolving</h3>
      <h3>Form: (status and errors are stores...)</h3>
      <textarea class="h-60 mb-8 w-full">${JSON.stringify(form, null, 2)}</textarea>
    `
    }

    return `
      <h3>Form Data</h3>
      <pre class="mb-8 overflow-y-auto">${JSON.stringify(form.data, null, 2)}</pre>
      <h3>Form: (status and errors are stores...)</h3>
      <textarea class="h-60 mb-8 w-full">${JSON.stringify(form, null, 2)}</textarea>
    `
  }

  function defaultReset (obj = {}) {
    const defaults = { string: '', number: 0, boolean: false }

    return Object.entries(obj).reduce((acc, val) => {
      let [key, value] = val
      value = Array.isArray(value) ? [] : defaults[typeof value]
      return { ...acc, [key]: value }
    }, {})
  }

  function useForm (spec) {
    const dispatch = createEventDispatcher()
    const status = writable(null)
    const errors = writable(null)

    const schema = {
      id: '',
      debug: false,
      finishDelay: 1200,
      initialData: null,
      undo: defaultReset,
      submit: (data, form, event) => {
        dispatch('form-submit', { data, form, event })
      },
      duringSubmit: (data, form, event) => {
        dispatch('form-during-submit', { data, form, event })
      },
      afterSuccess: (result, form, event) => {
        dispatch('form-success', { result, form, event })
      },
      afterChange: (changed, form) => {
        dispatch('form-change', { changed, form })
      },
      beforeChange: (changed, form) => {
        return { changed, form }
      },
      afterFinish: (outcome, form, event) => {
        dispatch('form-finish', { outcome, form, event })
      },
      afterErrors: (errors, form, event) => {
        dispatch('form-errors', { errors, form, event })
      },
      reset: (undo, form, event) => {
        dispatch('form-reset', { undo, form, event })
      }
    }

    const {
      id,
      debug,
      finishDelay,
      initialData,
      undo,
      reset,
      submit,
      beforeStart,
      beforeSubmit,
      duringSubmit,
      afterSuccess,
      afterErrors,
      beforeFinish,
      afterFinish,
      beforeChange,
      afterChange
    } = { ...schema, ...spec }

    const form = function form (node, data) {
      form.el = node
      form.data = data
      form.id = id || node.id
      // TODO: grab the submit button so we can auto disable it

      if (debug) {
        form.el.insertAdjacentHTML('beforeend', '<div id="form-debug" class="border border-blue-500 my-4 p-4"></div>')
        form.debugEl = document.getElementById('form-debug')
        form.debugEl.innerHTML = getDebug(form)
      }

      form.submitEvent = async (event) => {
        event.preventDefault()
        status.update(() => 'starting')
        errors.update(() => {})
        beforeStart && beforeStart(form, event)

        if (!validateForm(form.el)) {
          form.is.valid = false
          console.log('invalid form')
        }

        beforeSubmit && beforeSubmit(form, event)
        status.update(() => 'submitting')

        form.outcome = null
        try {
          form.afterwards = duringSubmit && duringSubmit(data, form, event)
          form.result = await submit(data, form, event)
          form.afterwards && form.afterwards(data, form, event)
          form.afterSubmit && form.afterSubmit(data, form, event)
          form.outcome = 'success'
          afterSuccess(form.result, form, event)
        } catch (errs) {
          if (debug) {
            console.log({ errs })
          }

          form.afterwards && form.afterwards()
          errors.update(() => errs.data || {})
          afterErrors(errs.data || errs, form, event)
          form.outcome = 'errors'
        } finally {
          status.update(() => 'finishing')
          beforeFinish && beforeFinish(form.outcome, form, event)
          afterFinish && afterFinish(form.outcome, form, event)
        }

        await setTimeout(() => { status.update(() => null) }, finishDelay)
      }

      form.el.addEventListener('submit', form.submitEvent)

      form.resetEvent = async (event) => {
        event.preventDefault()
        await reset(form.undo, form, event)
      }

      form.el.addEventListener('reset', form.resetEvent)

      return {
        update (updated) {
          beforeChange(updated, form)

          form.data = updated

          if (form.debugEl) {
            form.debugEl.innerHTML = getDebug(form)
          }


          afterChange(updated, form)
        },
        destroy () {
          form.el.removeEventListener('submit', form.submitEvent)
          form.el.removeEventListener('reset', form.resetEvent)
        }
      }
    }

    // Other props
    form.id = id
    form.status = null
    form.initialData = initialData
    form.undo = (obj) => undo(obj || form.initialData || {})
    form.validate = () => validateForm(form.el)
    form.is = {
      valid: false
    }

    form.toJSON = () => ({
      id: form.id,
      status: form.status,
      valid: form.is.valid,
      errors: form.errors,
      submit: submit.printToString(),
      reset: reset.printToString(),
      initialData: form.initialData,
      afterErrors: afterErrors.printToString(),
      afterChange: afterChange.printToString()
    })

    return { form, status, errors }
  }
</script>