<script> export let data </script>

<ul>
	{#each data as d}
	<li><pre>{JSON.stringify(d,null,2)}</pre></li>
	{/each}
</ul>
