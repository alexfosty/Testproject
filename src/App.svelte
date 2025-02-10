<script lang="ts">
  import { moodData } from './lib/MoodData';
  import { fade, fly, scale } from 'svelte/transition';
  import { elasticOut } from 'svelte/easing';
  
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
  <div class="text-center mb-5" in:fade={{ duration: 800 }}>
    <h1 class="display-3 fw-bold text-primary mb-3">MoodFlip 🔄</h1>
    <p class="lead fs-4 text-secondary">Turn that frown upside down!</p>
    <div class="border-bottom border-primary w-25 mx-auto my-4"></div>
  </div>

  {#if selectedMood === null}
    <div class="row g-4">
      {#each moodData as mood, index}
        <div class="col-md-6"
          in:fly={{ y: 50, duration: 500, delay: index * 100 }}>
          <div class="card h-100 shadow-sm hover-card">
            <div class="card-body d-flex flex-column align-items-center justify-content-between p-4">
              <p class="card-text fs-5 mb-4">{mood.negative}</p>
              <button 
                class="btn btn-primary btn-lg px-4 rounded-pill"
                on:click={() => selectedMood = index}>
                Select
              </button>
            </div>
          </div>
        </div>
      {/each}
    </div>
  {:else}
    <div in:scale={{ duration: 400, easing: elasticOut }}>
      <div class="card shadow-lg">
        <div class="card-body p-5 text-center">
          <h5 class="card-title fs-4 mb-4">
            {moodData[selectedMood].negative}
          </h5>
          
          {#if showPositive}
            <div class="alert alert-success shadow-sm my-4" 
              in:fly={{ y: 20, duration: 400 }}>
              <h4 class="alert-heading mb-3">Here's your flip! 🎉</h4>
              <p class="fs-5">{moodData[selectedMood].positive}</p>
            </div>
            <button class="btn btn-outline-secondary btn-lg px-4 rounded-pill" 
              on:click={reset}>
              Start Over
            </button>
          {:else}
            <button class="btn btn-primary btn-lg px-5 py-3 rounded-pill" 
              on:click={flipMood}>
              Flip it! 🔄
            </button>
          {/if}
        </div>
      </div>
    </div>
  {/if}
</main>

<style>
  :global(body) {
    background-color: #f8f9fa;
    min-height: 100vh;
  }

  .hover-card {
    transition: transform 0.2s ease-in-out;
  }

  .hover-card:hover {
    transform: translateY(-5px);
  }

  .btn {
    transition: all 0.2s ease-in-out;
  }

  .btn:hover {
    transform: scale(1.05);
  }

  .card {
    border-radius: 1rem;
    border: none;
  }

  .alert {
    border-radius: 1rem;
  }
</style>