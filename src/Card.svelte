<script lang='ts'>
  import Up from './ArrowUp.svelte';
  import Down from './ArrowDown.svelte';
  import Delete from './Delete.svelte';

  export let card: CardType;
  export let remove: () => void;
  export let incrementValue: () => void;
  export let decrementValue: () => void;
</script>

<div class="border-2 border-solid border-black rounded-lg flex flex-col justify-center items-center relative p-6 m-4">
  <button on:click={incrementValue}>
    <Up />
  </button>

  <div class="text-black">{card.value}</div>

  <button on:click={decrementValue}>
    <Down />
  </button>

  <button on:click={remove} class="absolute z-20 top-0 right-0">
    <Delete />
  </button>
</div>