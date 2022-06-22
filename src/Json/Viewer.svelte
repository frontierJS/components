<script>
  import { createEventDispatcher } from 'svelte'
  export let json = undefined
  export let name = undefined
  export let value = undefined
  export let title = undefined
  export let editFeature = false

  const dispatch = createEventDispatcher()
  let edit = false

  function handleClick(e) {
    const classes = Array.from(e.target.classList)
    if (
      edit &&
      classes.length &&
      !(
        classes.includes('value') ||
        classes.includes('edit-value') ||
        classes.includes('editting-value')
      )
    ) {
      finishEditting()
    }
  }

  function handleKeydown(e) {
    if (e.key === 'Enter') {
      e.preventDefault()
      finishEditting()
    }
  }

  function finishEditting() {
    edit = false
    dispatch('change', { value })
  }

  function editValue(e) {
    edit = true
  }
</script>

<svelte:body on:click={handleClick} />

<div class={$$restProps.class}>
  {#if title}
    <h3 class="my-4 font-bold">{title}</h3>
  {/if}

  {#if json && typeof json === 'object'}
    {#each Object.keys(json) as n}
      <svelte:self name={n} bind:value={json[n]} {editFeature} on:editted>
        <slot {value} />
      </svelte:self>
    {/each}
  {:else if name}
    <div class="setting">
      <div class="name">
        {name || 'json'}
      </div>
      <div class="value" on:click={editValue}>
        {#if value && Array.isArray(value)}
          {#each value as n, index (index)}
            <svelte:self name={String(index)} bind:value={n} {editFeature} />
          {/each}
        {:else if value && typeof value === 'object'}
          {#each Object.keys(value) as n}
            <svelte:self name={n} bind:value={value[n]} {editFeature} />
          {/each}
        {:else if editFeature && edit}
          <!-- svelte-ignore a11y-autofocus -->
          <textarea
            autofocus
            class="editting-value text-sm h-8 p-0 px-2"
            on:keydown={handleKeydown}
            on:blur={finishEditting}
            name="currentValue"
            id=""
            bind:value />
        {:else}
          <span class="edit-value" class:cursor-pointer={editFeature}>
             {value || ''}
          </span>
        {/if}
      </div>
    </div>
  {/if}
</div>

<style>
  .setting {
    display: grid;
    grid-auto-flow: column;
    grid-template-columns: minmax(75px, min-content) 5fr;
    border: 1px solid #f5f5f5;
    font-size: 0.8rem;
  }

  .name {
    background: #f5f5f5;
    padding: 5px;
    font-weight: bold;
    border: 1px solid white;
    white-space: nowrap;
  }

  .value {
    background: #fff;
    min-width: 100px;
    word-break: break-all;
    padding: 5px;
  }
</style>
