<script context="module">
  export { TableRow, Search, Sort }
  export const setSearchLabels = _setSearchLabels

  import TableRow from "./table-row.svelte"
  import Search, { setLabels as _setSearchLabels } from "./search.svelte"
  import Sort from "./sort.svelte"
  import { Table } from 'sveltestrap'
  import { SveltestrapPagination } from 'sveltestrap-paginate'
</script>

<script>
  import { createEventDispatcher, setContext } from "svelte"
  const dispatch = createEventDispatcher()
  export let loading = false
  export let page = 1
  export let currentPage = 1
  export let pageSize = 10
  export let rows
  export let size = false
  export let responsive = true
  export let bordered = false
  export let borderless = false
  export let striped = false
  export let dark = false
  export let hover = false
  export let totalItems = 0

  let buttons = [-2, -1, 0, 1, 2]
  let pageCount = 0

  setContext("state", {
    getState: () => ({
      page,
      currentPage,
      pageSize,
      rows,
    }),
    setPage: (_page, _currentPage) => {
      page = _page
      currentPage = _currentPage
    },
  });

  function onPageChange(event) {
    dispatch("pageChange", event.detail)
    currentPage = event.detail.page
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
    <slot />
    <slot name="foot" />
  </Table>

  <slot name="bottom">
    <div class="slot-bottom">
      <SveltestrapPagination
        {totalItems}
        currentPage="{currentPage}"
        pageSize="{pageSize}"
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