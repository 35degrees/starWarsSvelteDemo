<script>
  export let movie
</script>


  <div class="container">
    <img src="{movie.poster_path}" alt="{movie.title} poster" />
    <h2>{movie.title} <span class="score" class:high={movie.vote_average > 8} class:mid={movie.vote_average <= 8 && movie.vote_average > 6} class:low={movie.vote_average <= 6}>({movie.vote_average})</span></h2>
    <p class="release">{movie.release_date}</p>
    <div class="buttons">
      <slot />
    </div>
  </div>



<style>
  img {
    width: 100%;
    position: relative;
  }
h2 {
    margin: 0;
    font-size: 1.2rem;
    font-weight: 400;
  }

  .container {
    position: relative;
    max-width: 180px;
    margin: 0 auto;
  }

  .container:hover .buttons,
  .container:focus-within .buttons {
    opacity: 1;
  }

  .buttons {
    opacity: 0;
    position: absolute;
    bottom: 4.2rem;
    left: 0.5rem;
  }

  .release {
    position: absolute;
    top: 0;
    right: 0;
    left: 0;
    padding: 0.3rem;
    color: white;
    background-color: rgba(0, 0, 0, 0.5);
  }

  .score {
    font-size: 0.8em;
    font-weight: 700;
  }

  .high {
    color: forestgreen;
  }

  .mid {
    color: goldenrod;
  }

  .low {
    color: crimson;
  }


</style>