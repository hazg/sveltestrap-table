<script context="module">
  export { TableRow, Search, Sort }
  export const setSearchLabels = _setSearchLabels
  export const setSortLabels = _setSortLabels

  import TableRow from "./table-row.svelte"
  import Search, { setLabels as _setSearchLabels } from "./search.svelte"
  import Sort, { setLabels as _setSortLabels } from "./sort.svelte"
  import { Table } from 'sveltestrap'
  import { SveltestrapPagination } from 'sveltestrap-paginate'
</script>

<script>
  import { createEventDispatcher, setContext } from "svelte"
  const dispatch = createEventDispatcher()
  export let loading = false
  export let page = 0
  export let currentPage = 0
  export let pageSize = 10
  export let rows
  export let size = false
  export let responsive = true
  export let bordered = false
  export let borderless = false
  export let striped = false
  export let dark = false
  export let hover = false

  let buttons = [-2, -1, 0, 1, 2]
  let pageCount = 0

  $: filteredRows = rows
  $: visibleRows = filteredRows.slice(currentPage, currentPage + pageSize)

  setContext("state", {
    getState: () => ({
      page,
      currentPage,
      pageSize,
      rows,
      filteredRows
    }),
    setPage: (_page, _currentPage) => {
      page = _page
      currentPage = _currentPage
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
      <SveltestrapPagination
        totalItems="{rows.length}"
        pageSize="{pageSize}"
        currentPage="{currentPage}"
        limit="{2}"
        showStepOptions="{true}"
        on:setPage={onPageChange}
      />
    </div>
  </slot>
</div>

<style>
  .loading{
    pointer-events: none;
    opacity: 0.5;
  }
</style>