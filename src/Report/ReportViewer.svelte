<script>
  import { onMount } from 'svelte'
  import { views } from '@/resources/Report.svelte'
  import ExportLink from '@/components/ExportLink.svelte'

  export let report = undefined
  export let exportFeature = true

  onMount(() => { if (report.view) report.component = views[report.view] })

  function merge(template, data) {
    if (data.meta && typeof data.meta === 'string') {
      data.meta = JSON.parse(data.meta)
    }

    const pattern = /{\s*(\w+?)\s*}/g // {resource}
    function replacer (string, mergeData) {
      if (!string) return ''

      const replacedText = string.replace(pattern, (_, resource) =>  mergeData[resource] || '')

      return replacedText
    }

    return replacer(template, data)
  }
</script>

{#if report?.component}
  <svelte:component this={report.component} {report} data={report.data} let:item>
    {@html merge(report.meta?.slot, item)}
  </svelte:component>
  <br>
  {#if exportFeature}
    <ExportLink data={report?.data} />
  {/if}
{/if}
