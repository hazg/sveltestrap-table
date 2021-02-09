<script>
  import Icon from 'svelte-dynamic-iconify'
  import { createEventDispatcher } from "svelte"
  const dispatch = createEventDispatcher()

  export let dir = "none";
  export let key
  export let sortBy
  export let labels = {
    asc:      { title: "Ascending", icon: 'fa:sort-asc' },
    desc:     { title: "Desceding", icon: "fa:sort-desc" },
    unsorted: { title: "Unsorted",  icon: "fa:sort" },
  };

  function onClick(event) {

    const detail = {
      originalEvent: event,
      key,
      dir: dir !== "desc" ? "desc" : "asc",
    };

    dispatch("sort", detail)

    if (detail.preventDefault !== true) {
      dir = detail.dir
    }
    console.log(sortBy)
  }
</script>

<style>
  .sort {
    right: 0;
    cursor: pointer;
    /* position: absolute; */
    padding: 0 0.25em;
    color: #999;
  }
</style>

<span class="sort" on:click={onClick}>
  {#if dir === 'asc'}
    <Icon name={labels.asc.icon} />
  {:else if dir === 'desc'}
    <Icon name={labels.desc.icon} />
  {:else}
    <Icon name={labels.unsorted.icon} />
  {/if}
</span>
