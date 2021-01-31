<script context="module">
  // TODO: Convert custom pagination to sveltestrap
  let globalLabels;

  export function setLabels(labels) {
    globalLabels = labels;
  }
</script>

<script>
  import { Pagination, PaginationItem, PaginationLink } from 'sveltestrap';
  import { createEventDispatcher, getContext } from "svelte";
  const dispatch = createEventDispatcher();
  const stateContext = getContext("state");

  export let buttons = [-2, -1, 0, 1, 2];
  export let count;
  export let page = 0;
  export let pageSize;
  export let serverSide = false;

  export let labels = {
    first: "First",
    last: "Last",
    next: "Next",
    previous: "Previous",
    ...globalLabels
  };

  $: pageCount = Math.floor(count / pageSize);

  function onChange(event, page) {
    const state = stateContext.getState();
    const detail = {
      originalEvent: event,
      page,
      pageIndex: serverSide ? 0 : page * state.pageSize,
      pageSize: state.pageSize
    };
    dispatch("pageChange", detail);

    if (detail.preventDefault !== true) {
      stateContext.setPage(detail.page, detail.pageIndex);
    }
  }
</script>

<style>
</style>

<Pagination>
  <PaginationItem>
    <PaginationLink disabled={page === 0} on:click={e => onChange(e, 0)}>
      {labels.first}
    </PaginationLink>
  </PaginationItem>
  <PaginationItem>
    <PaginationLink disabled={page === 0} on:click={e => onChange(e, page - 1)}>
      {labels.previous}
    </PaginationLink>
  </PaginationItem>
  {#each buttons as button}
    {#if page + button >= 0 && page + button <= pageCount}
      <PaginationItem>
        <PaginationLink
          disabled={page === page + button}
          on:click={e => onChange(e, page + button)}>
          {page + button + 1}
        </PaginationLink>
      </PaginationItem>
    {/if}
  {/each}
  <PaginationItem>
    <PaginationLink
      disabled={page > pageCount - 1}
      on:click={e => onChange(e, page + 1)}>
      {labels.next}
    </PaginationLink>
  </PaginationItem>
  <PaginationItem>
    <PaginationLink disabled={page >= pageCount} on:click={e => onChange(e, pageCount)}>
      {labels.last}
    </PaginationLink>
  </PaginationItem>
</Pagination>
