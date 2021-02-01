<script>
  import { Pagination, PaginationItem, PaginationLink } from 'sveltestrap'
  import { createEventDispatcher, getContext } from "svelte"
  const dispatch = createEventDispatcher()
  const stateContext = getContext("state")

  export let buttons = [-2, -1, 0, 1, 2]
  export let count
  export let page = 0
  export let pageSize
  export let serverSide = false

  $: pageCount = Math.floor(count / pageSize)

  function onChange(event, page) {
    const state = stateContext.getState()
    const detail = {
      originalEvent: event,
      page,
      pageIndex: serverSide ? 0 : page * state.pageSize,
      pageSize: state.pageSize
    };
    dispatch("pageChange", detail)

    if (detail.preventDefault !== true) {
      stateContext.setPage(detail.page, detail.pageIndex)
    }
  }
</script>

<style>
</style>

<Pagination>
  <PaginationItem disabled={page === 0}>
    <PaginationLink first on:click={e => onChange(e, 0)}></PaginationLink>
  </PaginationItem>
  <PaginationItem disabled={page === 0}>
    <PaginationLink previous on:click={e => onChange(e, page - 1)}></PaginationLink>
  </PaginationItem>
  {#each buttons as button}
    {#if page + button >= 0 && page + button <= pageCount}
      <PaginationItem active={page === page + button}>
        <PaginationLink
          disabled={page === page + button}
          on:click={e => onChange(e, page + button)}>
          {page + button + 1}
        </PaginationLink>
      </PaginationItem>
    {/if}
  {/each}
  <PaginationItem disabled={page > pageCount - 1}>
    <PaginationLink next on:click={e => onChange(e, page + 1)} />
  </PaginationItem>
  <PaginationItem disabled={page >= pageCount}>
    <PaginationLink last on:click={e => onChange(e, pageCount)} />
  </PaginationItem>
</Pagination>
