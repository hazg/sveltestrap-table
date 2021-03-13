[Original source](https://svelte.dev/repl/3238e5737f764431a26e243800dccc6d?version=3.16.4)
---
Simple table wrapper with pagination and sort controls

dev version
===========
do not use in production

```svelte
<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.0.0-beta2/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-BmbxuPwQa2lc/FVzBcNJ7UAyJxM6wuqIj61tLrc4wSX0szH/Ev+nYRRuWlolflfl" crossorigin="anonymous">

<script>
	import SveltestrapTable, {TableRow} from 'sveltestrap-table'
	import dayjs from 'dayjs'

	let limit = 10
	let page = 1
	let apiUrl = 'https://604b7b98ee7cb900176a2338.mockapi.io/api/v1/sveltestrap-table?limit='+limit
	let data = getData()

  function getData() {
		fetch(apiUrl+'&'+'page='+ page)
			.then(response=>response.json())
			.then(d=>{ data = d })
  }

	function pageChange(e){
		console.log(e)
	}
</script>

{#if data}
	<SveltestrapTable on:pageChange={pageChange} totalItems=100>
		{#each data as row}
			<TableRow>
				<td>{row.id}</td>
				<td>{row.name}</td>
				<td>{dayjs(row.createdAt).format('MMMM D, YYYY h:mm A')}</td>
			</TableRow>
		{/each}
	</SveltestrapTable>
{/if}
```