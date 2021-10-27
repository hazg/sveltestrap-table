<script>
  import Icon from "@iconify/svelte"
  import { createEventDispatcher } from "svelte"

  const dispatch = createEventDispatcher()

  export let sortBy
  export let key
  export let icons = {
    asc:      'fa:sort-asc',
    desc:     'fa:sort-desc',
    unsorted: 'fa:sort'
  }

  let state = false

  function onClick(event) {
    state = state ? 0:1
    dispatch("sort", {key: key, dir: state?'asc':'desc'})
  }
</script>

<span
  class="sort"
  class:active={sortBy.key == key}
  class:asc={state}
  class:desc={!state}
  on:click={onClick}
>
  {#if sortBy.key == key}
    {#if state}
      <Icon icon={icons.asc} />
    {:else}
      <Icon icon={icons.desc} />
    {/if}
  {:else}
    <Icon icon={icons.unsorted} />
  {/if}
  <slot />
</span>

<style>
  .sort {
    right: 0;
    cursor: pointer;
    padding: 0 0.25em;
    color: #999;
  }
  .sort.active {
    color: black;
  }
</style>
