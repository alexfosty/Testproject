<script lang="ts">
  import { moodData } from './lib/MoodData';
  let selectedMood: number | null = null;
  let showPositive = false;

  function flipMood() {
    showPositive = true;
  }

  function reset() {
    selectedMood = null;
    showPositive = false;
  }
</script>

<main class="container py-4">
  <div class="text-center mb-4">
    <h1 class="display-4">MoodFlip 🔄</h1>
    <p class="lead">Turn that frown upside down!</p>
  </div>

  {#if selectedMood === null}
    <div class="row g-4">
      {#each moodData as mood, index}
        <div class="col-md-6">
          <div class="card h-100">
            <div class="card-body">
              <p class="card-text">{mood.negative}</p>
              <button 
                class="btn btn-primary"
                on:click={() => selectedMood = index}
              >
                Select
              </button>
            </div>
          </div>
        </div>
      {/each}
    </div>
  {:else}
    <div class="card text-center">
      <div class="card-body">
        <h5 class="card-title mb-4">
          {moodData[selectedMood].negative}
        </h5>
        
        {#if showPositive}
          <div class="alert alert-success">
            <h4 class="alert-heading">Here's your flip! 🎉</h4>
            <p>{moodData[selectedMood].positive}</p>
          </div>
          <button class="btn btn-secondary" on:click={reset}>
            Start Over
          </button>
        {:else}
          <button class="btn btn-primary btn-lg" on:click={flipMood}>
            Flip it! 🔄
          </button>
        {/if}
      </div>
    </div>
  {/if}
</main>

<style>
  :global(body) {
    background-color: #f8f9fa;
  }
</style>