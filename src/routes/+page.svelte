<script>
  import movies from '../lib/movies.json';
  import Movie from '../components/Movie.svelte';
  import { fade,slide,crossfade } from 'svelte/transition';
	import { quintOut } from 'svelte/easing'
  import {flip} from 'svelte/animate'

  const [send,receive] =crossfade({
    duration:500,
    easing: quintOut,
    fallback: (node) => {
      return fade(node,{duration:300})
    },
  })

  let filter = ''
  const sortOptions = ['title','release_date','vote_average']
  let sortBy = sortOptions[0]

  let selected = []
  
  $: sorted = sort(movies,sortBy)
  $: filtered = sorted.filter(movie=>movie.title.toLowerCase().includes(filter.toLowerCase()) && !selected.includes(movie))

function sort(list,property) {
  if (sortBy !== 'vote_average') {
    return [...list].sort((a,b)=> a[property] > b[property] ? 1 : -1)
  } else {
    return [...list].sort((a,b)=> a[property] < b[property] ? 1 : -1)
  }
}

function addMovie(movie) {

    selected.unshift(movie)
    selected = selected

}

function removeMovie(movie) {
  const index = selected.indexOf(movie)
  selected.splice(index,1)
  selected = selected
}
</script>


<div class="layout">
  <section>
    <h1>Star Wars {sortBy}</h1>
    <p>Create your own Star Wars movie watch order</p>
    <div>
      <label for="filter">Filter</label>
      <input type="text" id="filter" bind:value={filter}/>
      <label for="sort">Sort By</label>
      <select id='sort' bind:value={sortBy}>
        {#each sortOptions as option}
        <option>{option}</option>
        {/each}
      </select>
      <button class='reset' on:click={() => {
          selected = []
          filter = ''
      }}>Reset</button>
    </div>
    <ul class="all-movies">
      {#each filtered as movie (movie.id)}
      <li animate:flip={{duration:500}}
      in:receive={{key: movie.id}} out:send={{key: movie.id}}>
        <Movie {movie}>
            <button on:click={()=> addMovie(movie)}>Add Movie</button>
        </Movie>

      </li>
      {/each}

    </ul>
  </section>

  <section>
    <h2>Selected</h2>
    <ul class="selected">
      {#each selected as movie (movie.id)} 
      <li animate:flip={{duration: 300}}
      in:receive={{key: movie.id}} out:send={{key:movie.id}}>
        <p class="close" on:click={()=>removeMovie(movie)}>X</p>
        <Movie {movie} />

      </li>
      {/each}
    </ul>
  </section>
</div>

<style>


@import url('https://fonts.googleapis.com/css2?family=Poppins:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;0,800;1,400&display=swap');


  ul {
    list-style: none;
    padding: 0;

  }
  section {
    border: 1px dotted dodgerblue;
  }
  .all-movies {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
    list-style: none;
    gap: 1rem;
    text-align: center;
    justify-content: center;
    row-gap: 1.5rem;

  }


  .layout {
    display: grid;
    grid-template-columns: 1fr 20%;
    align-items: start;
    gap: 2rem;
    font-family: 'Poppins',sans-serif;
    text-align: center;
   
  }

  .selected {
    display: flex;
    flex-direction: column;
    gap: 1rem;
    align-items: center;
  }
  .movie-content {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    gap: 0.4rem;
  }


 .close {
  /* border: 1px solid green; */
  margin-bottom: -1.2rem;
  width: 112%;
  text-align: right;
  font-size: 1.1rem;
  z-index: -1;
  cursor: pointer;
 }
 .reset {
  background-color: transparent;
  border: 1px solid rebeccapurple;
  padding: 0.2rem 0.8rem;
  border-radius: 8px;
  margin-left: 0.8rem;
  color: rebeccapurple;
  text-transform: uppercase;
  letter-spacing: 1px;
  cursor: pointer;

 }
</style>