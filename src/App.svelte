<script>
  import { onMount } from 'svelte';
  import Pet from './components/Pet.svelte';
  import Controls from './components/Controls.svelte';
  import Stats from './components/Stats.svelte';

  let hunger = 0;
  let boredom = 0;
  let intellect = 0;
  let isAlive = true;
  let hasEscaped = false;
  
  const maxIntellect = 100;
  const hungerLimit = 10; // Define a hunger limit at which the pet dies

  function feedPet(amount) {
    hunger = Math.max(0, hunger - amount);
  }

  function playWithPet() {
    boredom = 0;
    // Ask a question (optional logic to implement)
  }

  function trainPet() {
    if (intellect < maxIntellect) {
      intellect += 10;
    }
  }

  // Hunger and boredom increase over time
  onMount(() => {
    const hungerInterval = setInterval(() => {
      if (isAlive && !hasEscaped) {
        hunger += 1;
        if (hunger >= hungerLimit) {
          isAlive = false;
        }
      }
    }, 1000);

    const boredomInterval = setInterval(() => {
      if (isAlive && !hasEscaped) {
        boredom += 1;
        if (boredom >= 10) {
          hasEscaped = true;
        }
      }
    }, 1000);

    return () => {
      clearInterval(hungerInterval);
      clearInterval(boredomInterval);
    };
  });
</script>

<main>
  {#if isAlive && !hasEscaped}
    <Pet hunger={hunger} boredom={boredom} intellect={intellect} />
    <Controls onFeed={feedPet} onPlay={playWithPet} onTrain={trainPet} />
    <Stats hunger={hunger} boredom={boredom} intellect={intellect} />
  {/if}

  {#if !isAlive}
    <h1>Ваш питомец умер из-за голода!</h1>
  {/if}

  {#if hasEscaped}
    <h1>Ваш питомец убежал из-за скуки!</h1>
  {/if}
</main>