<script context="module">
  export { TableRow, Search, Sort }
  export const setSearchLabels = _setSearchLabels
  export const setSortLabels = _setSortLabels

  import TableRow from "./table-row.svelte"
  import Search, { setLabels as _setSearchLabels } from "./search.svelte"
  import Sort, { setLabels as _setSortLabels } from "./sort.svelte"
  import { Table } from 'sveltestrap'
</script>

<script>
  import { createEventDispatcher, setContext } from "svelte"
  const dispatch = createEventDispatcher()

  export let loading = false
  export let page = 0
  export let pageIndex = 0
  export let pageSize = 10
  export let rows
  export let serverSide = false
  export let size = false
  export let responsive = false
  export let bordered = false;
  export let borderless = false;
  export let striped = false;
  export let dark = false;
  export let hover = false;

  let buttons = [-2, -1, 0, 1, 2]
  let pageCount = 0

  $: filteredRows = rows
  $: visibleRows = filteredRows.slice(pageIndex, pageIndex + pageSize)

  setContext("state", {
    getState: () => ({
      page,
      pageIndex,
      pageSize,
      rows,
      filteredRows
    }),
    setPage: (_page, _pageIndex) => {
      page = _page
      pageIndex = _pageIndex
    },
    setRows: _rows => (filteredRows = _rows)
  });

  function onPageChange(event) {
    dispatch("pageChange", event.detail)
  }

  function onSearch(event) {
    dispatch("search", event.detail)
  }
</script>


<div class="sveltestrap-table" class:loading>
  <slot name="top">
    <div class="slot-top">
      <svelte:component this={Search} on:search={onSearch} />
    </div>
  </slot>

  <Table
    {responsive}
    {size}
    {bordered}
    {borderless}
    {striped}
    {dark}
    {hover}
  >
    <slot name="head" />
    <slot rows={visibleRows} />
    <slot name="foot" />
  </Table>

  <slot name="bottom">
    <div class="slot-bottom">
      <svelte:component
        this={Pagination}
        {page}
        {pageSize}
        {serverSide}
        count={filteredRows.length - 1}
        on:pageChange={onPageChange} />
    </div>
  </slot>
</div>

<style>
  .loading{
    pointer-events: none;
    opacity: 0.5;
  }
</style>