<script>
  export let onFeed;
  export let onPlay;
  export let onTrain;
  
  function feed() {
    const randomAmount = Math.floor(Math.random() * 10) + 1;
    onFeed(randomAmount);
  }
</script>

<div>
  <button on:click={feed}>Покормить</button>
  <button on:click={onPlay}>Играть</button>
  <button on:click={onTrain}>Учить</button>
</div>