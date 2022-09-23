<script context="module">
  import { exportCSV } from '../ExportLink.svelte'

  export { empty, exportCSV }

  function empty (node, options) {
    const html = `
      <tr class="bg-gray-200 h-30">
        <td colspan="${options.span || 1}">No ${options.name} found...</td>
      </tr>
    `
    node.insertAdjacentHTML('beforeend', html)
  }
</script>

<div class="overflow-x-auto {$$restProps.class || ''}">
  <table class="table-view">
    <thead class="bg-gray-100">
    <slot name="thead"></slot>
    </thead>
    <tbody class="divide-y bg-white divide-gray-200">
    <slot/>
    </tbody>
  </table>
</div>