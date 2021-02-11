<script context="module">
  export { TableRow, Sort }

  import TableRow from "./table-row.svelte"
  import Sort from "./sort.svelte"
  import { Table } from 'sveltestrap'
  import { SveltestrapPagination } from 'sveltestrap-paginate'
</script>

<script>
  import { createEventDispatcher } from "svelte"
  const dispatch = createEventDispatcher()
  export let loading = false
  export let currentPage = 1
  export let perPage = 10
  export let size = false
  export let responsive = true
  export let bordered = false
  export let borderless = false
  export let striped = false
  export let dark = false
  export let hover = false
  export let totalItems = 0

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
        {currentPage}
        {perPage}
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