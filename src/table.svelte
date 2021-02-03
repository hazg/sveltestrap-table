<script context="module">
  import Pagination from "./pagination.svelte"
  import TableRow from "./table-row.svelte"
  import Search, { setLabels as _setSearchLabels } from "./search.svelte"
  import Sort, { setLabels as _setSortLabels } from "./sort.svelte"
  export { Pagination, TableRow, Search, Sort }
  import { Table } from 'sveltestrap'

  export const setSearchLabels = _setSearchLabels
  export const setSortLabels = _setSortLabels
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
  <!-- class={'table ' + $$props.class} class:responsive -->
  <Table responsive>
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

  /*.table {
    width: 100%;
    border-collapse: collapse;
  }

  .table :global(th, td) {
    position: relative;
  }

  .table :global(td) {
    padding: 0.3em 0.3em;
  }

  .center {
    text-align: center;
    font-style: italic;
  }

  .center > span {
    padding: 10px 10px;
    float: left;
    width: 100%;
  }

  .slot-top,
  .slot-bottom {
    float: left;
    width: 100%;
    margin-top: 1em;
  }

  @media screen and (max-width: 767px) {
    table.responsive {
      border: 0;
    }

    table.responsive :global(thead) {
      border: none;
      clip: rect(0 0 0 0);
      height: 1px;
      margin: -1px;
      overflow: hidden;
      padding: 0;
      position: absolute;
      width: 1px;
    }

    table.responsive :global(tr) {
      border-bottom: 2px solid #ddd;
      display: block;
      padding-bottom: 0.3em;
      margin-bottom: 0.3em;
    }

    table.responsive :global(td) {
      border-bottom: 1px solid #ddd;
      display: block;
      font-size: 0.8em;
      text-align: right;
    }

    table.responsive :global(td::before) {
      /*
	* aria-label has no advantage, it won't be read inside a table content: attr(aria-label);
	*
      content: attr(data-label);
      float: left;
      font-weight: bold;
    }

    table.responsive :global(td[data-label-normal]::before) {
      font-weight: normal;
    }

    table.responsive :global(td[data-label-upper]::before) {
      text-transform: uppercase;
    }

    table.responsive :global(td:last-child) {
      border-bottom: 0;
    }
  }*/
</style>