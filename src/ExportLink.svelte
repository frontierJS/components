<script context="module">
  import { unparse } from 'papaparse'

  function parseData(data) {
      let exportData = data.map((item) => {
        return Object.entries(item).reduce((acc, [key, value]) => {
          if (typeof value === 'object') {
            acc[key] = JSON.stringify(value)
            // return  parseData(value)
          } else {
            acc[key] = value
          }
          return acc
        }, {})
      })
      return exportData
  }

  export function exportCSV (node, options) {
    const exportAction = () => {
      let exportData = parseData(options.data)
      const csv = unparse(exportData)
      node.setAttribute('download', options.file)
      node.href = window.URL.createObjectURL(
        new Blob([csv], { type: 'text/csv' })
      )
    }

    node.addEventListener('click', exportAction)

    return {
      update (newOptions) {
        options = newOptions
      },
      destroy () {
        node.removeEventListener('click', exportAction)
      }
    }
  }
</script>

<script>
  export let data = undefined
  export let text = 'Export'
  export let title = 'Export data to file'
  export let file = 'data.csv'
</script>

{#if data?.length}
  <a class="export-link {$$restProps.class}" href="#export" {title} use:exportCSV={{ data, file }} >{text}</a>
{/if}
