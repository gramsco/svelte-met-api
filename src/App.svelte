<script>
    
	import axios from 'axios'
  
	const MET_API = 'https://collectionapi.metmuseum.org/public/collection/v1/objects'
  
	const base = axios.create({baseURL:MET_API})
  
	const LOADING = "wait..."

	let data = {}
  
	let getArticle = (n) => base.get(`/${n}`)
  
	let selected = null
  
	let id = null

	async function handleClick(){
	  if (!!id) clearTimeout(id)
	  data = LOADING
	  let random = Math.floor(Math.random() * 47_4000)
	  try{
	  let {data:article} = await getArticle(random)
	  if (!!article?.primaryImageSmall){
	  	data = article
	  }
	  else {
		  handleClick()
	  }
	  } catch(err){
		  handleClick()
	  }
	}
  
  </script>
  

  <style>

	  :global(body){
		  background:black;
		  color:white;
		  padding:2%;
	  }

	.container{
		display:flex;
		flex-wrap:wrap;
	}

	.infos{
		overflow:auto;
		max-width:40vw;
	}

	li{
		list-style: none;
		max-width:100%;
		overflow:auto;
	}

	main{
		text-align: center;
	}

	.met-logo{
		height:40px;
	}
	

  </style>
  <h1>

	<a href="https://metmuseum.github.io/">
		  <img class="met-logo" src="images/met.jpg" alt="Met logo"/>
	</a>
 </h1>
  {#if data !== "wait..."}
  <button on:click={handleClick}>Fetch new article</button>
  {/if}

  <div class="container">
	<main>
		
		{#if !!data.title}
		<h1>{data.title}</h1>
		{/if}
		{#if !!data.primaryImageSmall}
		<img alt={data.title} src={data.primaryImageSmall}/>
		{/if}
	</main>

		<div class="infos">
			<h2>{Object.keys(data).length} keys</h2>
			{#if data === LOADING}
			<p>{LOADING}</p>
			{/if}
	<ul>
		{#if data !== LOADING}
		{#each Object.keys(data) as d}
		{#if !!data[d]}
		<li>{`${d} : ${data[d]}`}</li>
		{/if}
		{/each}
		{/if}
	</ul>
</div>

</div>