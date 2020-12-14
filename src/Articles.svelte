<script>
    
  import axios from 'axios'
  import snarkdown from 'snarkdown'

  const MET_API = 'https://collectionapi.metmuseum.org/public/collection/v1/objects'

  axios.create({baseURL:MET_API})

  let articles = axios.get('https://leqoqo.herokuapp.com/articles').then(res => res.data)

  let selected = null

  function handleClick(d){
    selected = d
  }

</script>

<style>

  li{
    margin:2%;
    cursor:pointer;
  }

  .list_of_articles{
    position:fixed;
    width:400px;
    height:100vh;
    overflow:auto;
    top:0;
    right:0;
    background:white;
  }

  main{
    margin-top:20vh;
    width:80vw;
  }

</style>

<div>

<div class="list_of_articles">
{#await articles}
	<p>Loading</p>
    {:then ars}
    <ul>
    {#each ars as article}
		<li on:click={() => handleClick(article)}>{article.title}</li>
    {/each}
    </ul>
	{:catch err}
		<p>{err}</p>
{/await}

</div>

<main>
  {#if selected !== null}  
  <h1>{selected.title}</h1>
  <p>{@html snarkdown(selected.body)}</p>
  {:else}
  <p>Selecte un article</p>
  {/if}
  </main>
</div>